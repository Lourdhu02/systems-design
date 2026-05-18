# 12 — Cost, Multi-tenancy, Scaling cheat sheet

1. **Inference compute dominates the bill** (60-90% for online ML). Optimise it first.

2. **Cost per request is a first-class metric**, alongside latency and quality.

3. **Smaller model + more training tokens** is the inference-cost-friendly recipe (Chinchilla 2022 + post-Chinchilla over-training).

4. **MIG for multi-tenant GPU.** Hardware isolation, easy chargeback.

5. **Serverless inference wins at low utilisation (<30%)**; self-managed wins at high steady-state.

6. **Cost optimisation order:** reduce request count, smaller model, right-size GPU, spot for training, prompt cache + output cap for LLMs.

7. **Multi-tenancy concerns:** noisy-neighbour (MIG), fairness (queues), accounting (per-tenant telemetry), quota (hard caps).

8. **When not to use ML** is the most under-taught skill. A rules baseline at 85% often beats an ML system at 91% on TCO.

9. **Scaling laws:** Kaplan 2020 says bigger; Chinchilla 2022 says ~20 tokens / param; 2024-2026 production says over-train for inference economics.

10. **"Free" product decisions are not free.** Every "show recs on more pages" doubles a real bill.
