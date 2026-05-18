# 10 — Monitoring and Drift cheat sheet

1. **Three drifts:** data (P(X)), concept (P(Y|X)), label (P(Y)). Different fixes.

2. **Build the prediction log first.** Without it, every downstream observability tool is blind.

3. **Coverage SLI > availability.** A model returning a fallback default is "available" but silently broken.

4. **Quality SLO with auto-rollback** is the closest thing to a kill-switch for a regressing model.

5. **Drift is a leading indicator.** Notify, don't page. Quality breach is page-worthy.

6. **Calibration plots** catch the silent class of "scores no longer correspond to probabilities."

7. **Per-slice metrics** catch what aggregate metrics hide.

8. **Model cards and data cards** are mandatory in regulated environments and good hygiene everywhere.

9. **LLM observability is different:** track tokens in/out, cache hit, refusal rate, model-graded hallucination on samples.

10. **Alert on actionable symptoms.** No human can act on "feature drift index = 0.13"; they can act on "p99 latency > SLO."
