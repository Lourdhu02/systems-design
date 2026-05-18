# 04 — Serving cheat sheet

1. **Three serving shapes:** online (synchronous), batch (table-in, table-out), streaming (reacting to events). Picking the wrong one is the most expensive architecture mistake.

2. **Default model runtime in 2026:** Triton for heterogeneous GPU serving, vLLM for LLMs. Custom Python only for internal low-QPS services.

3. **Dynamic batching is mandatory on GPU.** Single-element batches waste 90% of the silicon. Wait 5-20 ms for a real batch.

4. **For LLMs: continuous batching, not dynamic.** New requests join the in-flight batch every decode step.

5. **Latency budget: 80% rule.** Plan to spend 80% of the SLO. Tail variance eats the rest.

6. **Parallelize independent reads.** Feature fetch and candidate retrieval run concurrently; budget becomes the max, not the sum.

7. **Rollout sequence:** shadow → canary 1/5/25/100% → A/B confirmatory. Auto-rollback on latency or quality breach.

8. **A canary without auto-rollback is a slow blue-green.** Write the criteria; commit them to code.

9. **GPU autoscaling pain:** node 60-300 s + model load 20-180 s. Use warm pools or predictive scaling for spikes.

10. **Edge inference** wins for latency, privacy, and per-inference cost; loses on update agility and model size. Use a hybrid when the trade-off is sharp.
