# 13 — Privacy, Fairness, Ethics pitfalls

1. **Promising "anonymised" data.** Anonymisation rarely holds. Promise specific protections you can deliver.

2. **DP-SGD as performance.** Bolted on with no measurement, harming utility for no real privacy gain.

3. **Fairness by aggregate accuracy.** Aggregate is fine, slice is broken; you'll be on the news.

4. **No erasure procedure.** GDPR / CCPA requests pile up; you cannot fulfil them.

5. **No audit log.** Regulator asks a question; you have no answer.

6. **High-risk system shipped without DPIA.** Process violation; product launch held.

7. **Federated learning without secure aggregation or DP.** Privacy theatre; gradients leak.

8. **Red-teaming as a one-time launch event.** Adversaries adapt; red-teaming must be continuous.

9. **Model card without per-slice metrics.** Aggregate hides the bias the card was supposed to surface.

10. **PII passing to a hosted LLM API with no DPA or redaction.** Regulator-discoverable; vendor-discoverable; eventually customer-discoverable.
