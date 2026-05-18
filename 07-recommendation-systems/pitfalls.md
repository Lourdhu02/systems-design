# 07 — Recommendation Systems pitfalls

1. **Single retriever.** Missing >30% of clicks that would have come from a different retrieval mode.

2. **In-batch negatives without log-q correction.** Popular items systematically under-recommended.

3. **Easy negatives only.** Model can't distinguish similar items; retrieval recall plateaus.

4. **No exploration.** Ranker collapses onto a narrow set of items; feedback loop closes; long tail starves.

5. **Click-only metric.** Optimising clicks alone over-promotes clickbait. Use engagement-weighted compound metrics.

6. **No diversity layer.** Top 20 are 17 variations of the same theme. Long-term engagement craters.

7. **Cold-start by accident.** New items have no signal; the system never surfaces them; they never get signal. Force exploration for new items.

8. **Online learning by default.** Adversarial feedback poisons weights in minutes. Frequent batch retrain almost always wins.

9. **Ranker eval that ignores slices.** Aggregate AUC up, cohort AUC down on the slice that drives revenue.

10. **Position bias in training labels.** Items at position 1 get more clicks for reasons unrelated to relevance. Use position-aware loss (e.g., counterfactual estimators).
