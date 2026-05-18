# 00 — Foundations cheat sheet

One printable page. The ten things that anchor everything else.

1. **An ML system is not an ML model.** If you swap the model for `return 0.5` and most of the architecture remains, you have a system. Most of the cost is the surrounding architecture (Sculley et al., NeurIPS 2015).

2. **The five properties:** latency, throughput, cost, freshness, accuracy. Pick the two that matter for your product; trade the rest.

3. **The lifecycle is a loop.** Data -> features -> training -> registry -> deploy -> serve -> logs -> monitoring -> data. Open loop = science. Closed loop = engineering.

4. **Three serving shapes:** batch (cheapest, stalest), online (most common, expensive), streaming (real-time signals, medium cost). Picking the wrong one is the most expensive architecture mistake.

5. **Train-serve skew before drift.** When offline AUC is 0.85 and online AUC is 0.71, the bug is almost always in feature plumbing, not in the data distribution. Log the served features and train on those.

6. **One feature definition, two execution backends.** Offline and online feature compute must be derived from one source.

7. **SLOs, not SLAs, change behavior.** A latency SLO without an error-budget policy is decoration.

8. **ML-specific SLOs you usually forget:** freshness, coverage, prediction quality, drift.

9. **Predictions are logged with the features used and the model version.** This is the only debug surface that survives every other failure.

10. **Models are versioned at the binary, not the code commit.** Two runs of the same code produce different models.
