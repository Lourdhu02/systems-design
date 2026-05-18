# 03 — Training Infra cheat sheet

1. **Parallelism pick by model size:** sub-1B = DDP, 1B-10B = FSDP, 10B-100B = FSDP+TP, 100B+ = 3D parallelism (TP+PP+DP).

2. **Memory budget (AdamW BF16):** parameters `2P` + gradients `2P` + optimizer state `8P` + activations `BSHLc`. Activations are usually the largest.

3. **Activation checkpointing** trades ~25-30% compute for big memory savings. Almost always worth it.

4. **GPU economics:** spot for pre-training (50-80% cheaper, need solid checkpointing); on-prem only if utilisation is >60% over 2+ years.

5. **Schedulers in 2026:** K8s + Kueue + KubeRay for cloud-native, Slurm for bare-metal HPC, Ray for distributed-Python workloads.

6. **Gang scheduling and topology awareness aren't optional.** Default K8s scheduler is bad at both.

7. **Checkpoint every 1-3% of compute.** Async / sharded checkpoints if your checkpoint is large.

8. **Elastic training** lets jobs continue with N-1 workers; assume hardware failures, don't engineer to avoid them.

9. **Dataloader is the silent bottleneck.** Use WebDataset / Mosaic Streaming. 4-8 workers per GPU, `pin_memory=True`, `non_blocking=True`.

10. **Reproducibility = same artefact within statistical noise.** Bit-exact reproducibility is a tax you don't need.
