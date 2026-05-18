# 03 — Training Infra exercises

---

### 1. DECISION. You're training a 13B-parameter transformer for instruction-following. Budget is 64 H100s for 14 days. Pick a parallelism strategy.

<details><summary>Solution</summary>

13B params at BF16 = 26 GB params + 26 GB grads + 104 GB optimizer state (AdamW). That doesn't fit on one H100. The decision is between FSDP and FSDP + TP.

64 H100s spread across (typically) 8 nodes of 8 GPUs each. NVLink within node, InfiniBand or RoCE across.

**FSDP-only.** Shard params + grads + optimizer state across all 64 ranks. Simple. Communication is all-gather + reduce-scatter every step. At 64 ranks this is workable but the per-step all-gather is expensive.

**FSDP across nodes + TP within node.** TP-2 or TP-4 within each node; FSDP across the resulting (64/TP) ranks. Reduces FSDP shard count (cheaper all-gather) at the cost of intra-node tensor-parallel all-reduce.

For 13B: FSDP across all 64 ranks is usually fine. Add TP if you're hitting all-gather bottlenecks or if you scale up.

Use BF16, activation checkpointing on every layer, AdamW. Expect ~40-50% MFU; ~140B tokens in 14 days at that scale is plausible.
</details>

---

### 2. DECISION. Same 13B model, but the team is on a budget. Spot or on-demand?

<details><summary>Solution</summary>

Spot, with eyes open. Spot pricing is typically 50-70% of on-demand for H100s, which on a 14-day run is real money.

What you must have:

1. **Checkpointing every 30-60 minutes.** Async, sharded so it doesn't stall training.
2. **Elastic resume.** When a spot instance is preempted, the job picks up from the latest checkpoint on the remaining workers (or new workers as they get scheduled).
3. **Distributed checkpoint** (PyTorch DCP, 2023+) so checkpoint write is parallel and fast.
4. **A monitor that detects preemption signals** and triggers a clean checkpoint before eviction.

If preemption rate is >5% per hour, spot starts losing the math because lost work approaches savings. Most clouds publish preemption rates by region/instance; check before committing.

For a one-off 14-day run, spot is usually worth it. For a steady-state continuous training pipeline, mixed reserved + on-demand wins.
</details>

---

### 3. INTERVIEW. The eng manager asks: "We have 1024 H100s. How do we make sure we get >40% MFU?"

<details><summary>Solution</summary>

The relevant levers, in order:

1. **Avoid 3-D parallelism if the model doesn't need it.** Each axis of parallelism adds communication cost. Pure FSDP at thousand-GPU scale can sustain ~40% MFU on 7B-13B models.
2. **Activation checkpointing on the right layers.** Recompute heavy attention, keep cheaper MLPs cached.
3. **Topology-aware scheduling.** Co-locate workers that need NVLink (TP ranks) and rack-local InfiniBand (PP ranks).
4. **Fused kernels.** FlashAttention 2/3 (Dao 2022-2024), fused AdamW, fused LayerNorm. These move you from ~30% to ~45% MFU on transformers.
5. **Sequence packing.** Don't pad sequences to the max; pack multiple short sequences per microbatch.
6. **FP8 on Hopper / Blackwell** for the compute-bound layers if numerics are stable.
7. **Reduce communication** with sequence parallel + interleaved pipeline schedule (1F1B vs all-forward-all-backward).

A 40% MFU target is realistic; 50% is excellent; 55%+ is the territory of teams that publish about it.
</details>

---

### 4. CASE-STUDY READ. Read the Megatron-LM paper (Shoeybi et al., NVIDIA, 2019). What is the one trick that makes tensor parallel cheap inside a transformer?

<details><summary>Solution</summary>

**Splitting the matmul along the hidden dimension in a way that the column-split and row-split can be paired** so that the all-reduce happens only at the end of each transformer block, not after every matmul.

Concretely: for an MLP block `Y = GeLU(X * W1) * W2`, split `W1` column-wise (each rank holds a column slice) and `W2` row-wise (each rank holds a row slice). The intermediate activation is sharded across ranks; the final output requires an all-reduce of the partial sums. That's one all-reduce per MLP block.

The same trick applies to multi-head attention: split the heads across ranks, all-reduce after the output projection.

This is the single algorithmic insight that made TP practical inside transformers. Earlier "split this matmul across GPUs" approaches paid an all-reduce per operation, which is far too expensive.
</details>

---

### 5. INTERVIEW. Your team's pre-training runs lose ~5% of wall time to "stalls" — periods where GPU utilisation drops below 20%. How do you diagnose?

<details><summary>Solution</summary>

In order:

1. **Profile a single step.** PyTorch profiler or NSys. Look for gaps between forward and backward, or between backward and optimizer step.
2. **Dataloader.** `nvidia-smi dmon` while training. If GPU util oscillates, the loader is starving the GPU. Increase `num_workers`; check that workers aren't blocked on I/O (e.g., a too-slow object store, a tarball decode bottleneck).
3. **Network.** Is communication the bottleneck? Profile NCCL collectives. Check that you're using the right NCCL_ALGO / NCCL_PROTO for your interconnect.
4. **Stragglers.** In multi-node, one slow worker stalls the whole gang. Look at per-rank step time variance.
5. **Checkpoint stalls.** If checkpoints are synchronous and large, they show as periodic gaps. Switch to async / sharded.
6. **Hardware issues.** A degraded GPU (link errors, thermal throttling) silently kills perf. Check ECC counters and NCCL warnings.

The first three account for >80% of cases.
</details>

---

### 6. DECISION. You're choosing between Slurm and Kubernetes for a new on-prem GPU cluster (256 H100s). What do you pick?

<details><summary>Solution</summary>

Hinges on team's existing skill and serving stack.

**Slurm** if: the org has Slurm experience, jobs are primarily tightly-coupled pre-training runs, and serving uses a different cluster anyway. Slurm's gang scheduling and topology awareness are mature.

**Kubernetes** if: the org's serving stack is already K8s, you want one control plane, and you want a richer multi-tenant story. Use Kueue or Volcano for gang scheduling; the default K8s scheduler is inadequate.

For a fresh build in 2026 with a serving-driven org: Kubernetes + Kueue. For a research lab whose jobs are pure pre-training: Slurm.

The biggest mistake either way is **trying to make Slurm and K8s share GPUs**. Pick one for the cluster and stick to it.
</details>

---

### 7. INTERVIEW. Explain what FSDP does on each training step, in five sentences.

<details><summary>Solution</summary>

Each rank holds a shard of the model's parameters, gradients, and optimizer states. On the **forward pass**, each layer's parameters are all-gathered across ranks just-in-time, used for the forward computation, and then dropped. On the **backward pass**, the same all-gather happens to compute gradients; the gradients are then reduce-scattered so each rank holds its own shard of the gradient. The **optimizer step** runs locally on each rank, updating only its parameter shard. FSDP is conceptually equivalent to ZeRO-3 (Rajbhandari et al., 2020).
</details>

---

### 8. CASE-STUDY READ. Read PyTorch FSDP paper (Zhao et al., Meta, 2023). What were the two biggest practical limitations of the first FSDP implementation?

<details><summary>Solution</summary>

1. **Communication overhead** dominated at very large scales because every layer triggered an all-gather. Mitigation: per-parameter sharding (rather than per-module), pre-fetching the next layer's parameters during the current layer's compute, and overlap of all-gather with computation.
2. **Activation memory** wasn't sharded by FSDP itself — only parameters / grads / optimizer states. Mitigation: combine FSDP with activation checkpointing (mandatory at scale) and with sequence parallel.

Both are still active areas of optimization in 2024-2026.
</details>

---

### 9. DECISION. You have a 70B-parameter model fine-tune. The team wants to LoRA it instead of full fine-tune. Spec the savings.

<details><summary>Solution</summary>

Full fine-tune of 70B in BF16 with AdamW:

| Bucket | Bytes |
|--------|-------|
| Parameters | 140 GB |
| Grads | 140 GB |
| Optimizer state | 560 GB |
| **Subtotal** | **840 GB** |
| Activations (small batch) | ~50-200 GB |

LoRA (Hu et al., 2021): freeze base params, train only low-rank adapters. With rank 16 across attention layers, trainable params ~0.1-1% of the base.

| Bucket | Bytes (LoRA r=16) |
|--------|---------------------|
| Frozen params (BF16, no grads/opt) | 140 GB |
| Trainable params | ~0.5 GB |
| Trainable grads | ~0.5 GB |
| Trainable optimizer state | ~2 GB |
| Activations | similar to full fine-tune |

Total fits comfortably on a single 8xH100 node, often a single H200 (for inference-style memory) with QLoRA (4-bit base, Dettmers et al., 2023).

The cost ratio: LoRA fine-tuning is typically **20-100x cheaper** than full fine-tune for the same target task with comparable quality on instruction-following.
</details>

---

### 10. INTERVIEW. Your training run failed at hour 47 (out of 200). What's the recovery procedure, and what would you have built differently?

<details><summary>Solution</summary>

Procedure:

1. Identify the latest valid checkpoint. If you checkpointed every hour, you lose <=1 hour.
2. Restart the job pointing at the checkpoint. The scheduler should auto-reschedule on the same topology (or as close as possible).
3. Verify the optimizer state and learning-rate schedule resumed correctly (not "from scratch with high LR").
4. Log the failure: GPU id, error class, NCCL warnings if any. Drives node retirement.

What you would have built differently:

- **Async sharded checkpoints** so the 1-hour cadence has near-zero overhead.
- **Elastic worker support** so a single-GPU failure shrinks the world size rather than killing the job.
- **Hot spares** for the largest runs, where the throughput cost of stop-restart is worse than the cost of idle standby GPUs.
- **Per-rank step-time monitoring** to catch the slow-rank precursor that often appears before a hardware failure.
</details>

---

### 11. INTERVIEW. The team is debating whether to buy 200 H100s or pay for a year of cloud equivalent. Frame the analysis.

<details><summary>Solution</summary>

Build a TCO model with these axes:

| Cost component | On-prem | Cloud reserved |
|----------------|---------|----------------|
| GPU capex | ~$30-40k/H100 | n/a |
| DC space + power + cooling | ~$0.5-1/W/year amortized | n/a |
| Networking (IB, NVLink switches) | substantial | bundled |
| Ops headcount | 1-2 FTE for 200 GPUs | 0.2 FTE |
| Software (cluster manager, sched) | self-built / paid | bundled |
| Utility / breakage | ~3-5% of capex/year | n/a |
| Cloud reserved | n/a | ~$2.20/hour x 24 x 365 x 200 ≈ $3.9M/year |

For ~$3.9M/year cloud, the equivalent on-prem capex at $35k/H100 is ~$7M, plus DC + ops + power.

Break-even is typically 18-30 months of >60% utilisation. For predictable continuous workloads, on-prem wins by year 3. For spiky use, cloud wins forever.

Hidden costs that bite on-prem: hardware obsolescence (Hopper today, Blackwell next year — your capex depreciates fast), supply-chain risk, recruiting DC ops talent.

A common middle path in 2024-2026: a small on-prem core for steady-state + burst into cloud for peak demand.
</details>

---

### 12. INTERVIEW. Draw the chart of "GPU hours vs wall time vs cost" for a fixed FLOP budget, and explain how a manager should think about it.

<details><summary>Solution</summary>

For a fixed FLOP budget `F`, the total GPU-hours is `F / (peak_FLOPS * MFU)` — **constant** in cluster size. The wall time is GPU-hours / cluster size.

```text
Cluster size   |  Wall time   |  Cost (assuming flat $/GPU-hour)
N              |  T           |  C
2N             |  T/2         |  C
4N             |  T/4         |  C
```

Cost is invariant in cluster size; wall time inverse-linear. So the manager's question isn't "which is cheaper" — they cost the same — it's **"how much wall time is worth, and what's the marginal cost of waiting?"**

Three considerations:

1. **Researcher iteration speed.** A 2-day run lets you iterate twice a week; a 14-day run, once a month. Researcher time is expensive.
2. **Failure cost.** A 14-day run has more chances to fail. A 2-day run on 7x the cluster failure-rate goes up linearly with cluster size.
3. **Real-time price drift.** Spot prices and GPU availability fluctuate. A 14-day plan is more exposed to price moves than a 2-day plan.

The right answer is usually "as many GPUs as the job can use efficiently, capped at the budget for the wall time you actually want."
</details>
