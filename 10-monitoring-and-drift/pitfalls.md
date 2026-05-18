# 10 — Monitoring and Drift pitfalls

1. **No prediction log.** Every downstream observability tool is blind.

2. **Aggregate metrics that hide slice failures.** Model "fine" overall, broken for a critical cohort.

3. **Drift detector paging.** Drift is a leading indicator; alert noisy = oncall blind.

4. **Stationary reference distribution.** Comparing Monday traffic to a weekend-heavy reference will fire false drift alerts.

5. **No coverage SLI.** Model server up, model silently not firing because features broke.

6. **No calibration plot.** Scores drift in distribution while the model "works"; downstream consumers using thresholds are broken.

7. **Quality SLO with no auto-rollback.** The breach is logged but the bad model keeps serving.

8. **Late-label confusion.** Quality SLO computed on insufficient labels; figure is misleading.

9. **Model cards as a one-time write.** Released, never updated; describes a model two architectures ago.

10. **LLM observability that misses cache-hit ratio.** Cost balloons, nobody knows because nobody's looking.
