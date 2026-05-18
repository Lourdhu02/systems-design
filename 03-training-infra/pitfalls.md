# 03 — Training Infra pitfalls

1. **Picking the most parallel strategy by default.** 3D parallelism is overkill for a 7B model. Each extra axis adds communication cost.

2. **No activation checkpointing.** Memory blows up; you're forced into a parallelism strategy you don't need.

3. **Synchronous, single-rank-writes checkpoints.** A 500 GB checkpoint takes minutes to write; every checkpoint stalls the job. Use async sharded.

4. **Network not topology-aware.** TP ranks across nodes instead of within a node turn NVLink-class jobs into Ethernet-class jobs. Co-locate.

5. **Dataloader with `num_workers=0`.** Single-threaded data loading. GPU sits idle. Use 4-8 workers per GPU and streaming shard formats.

6. **Reading millions of tiny files.** Random reads on object storage are slow. Shard into sequential tar/WebDataset chunks.

7. **No per-rank step-time monitoring.** A straggler rank slows the whole gang; you don't see it because aggregate metrics average it out.

8. **Bit-exact reproducibility as a goal.** Engineering effort enormous, value approximately zero. Same artefact within statistical noise is what you need.

9. **Treating spot as on-demand.** Pre-emption with no async checkpoint = lost work. Spot is great IF the job is robust.

10. **Buying H100s without a serving plan.** Pre-training is bursty; the same H100s are wrong for inference (where L40S / L4 / A10 / H200 with MIG often serve more useful work). Design training and serving capacity together.
