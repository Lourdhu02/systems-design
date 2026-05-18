# 00 — Foundations pitfalls

Ten mistakes engineers actually make at the foundations level. Each one I have seen in production at least twice.

1. **Optimizing accuracy without naming the trade.** "Make it better" is not a target. Pick the property pair (e.g., latency + accuracy) you're optimizing and announce what you're willing to lose.

2. **Mixing batch and online stacks in one service.** Two completely different code paths held together by a single API. The hybrid wants to be two services with a clear seam.

3. **No prediction log.** Without per-request features + model version + output, every production debug session starts blind. Build the log on day one.

4. **Believing the offline metric.** A 0.05 AUC lift on the eval set is not 0.05 AUC in production. Until you've seen it in a shadow deploy or A/B test, treat offline numbers as hypothesis-level.

5. **No model version in the request path.** Every prediction must record the exact model binary that produced it. Code commit is not enough — see "Models are versioned at the binary" in the cheat sheet.

6. **Latency SLO with no headroom.** A 200 ms SLO and a 195 ms p99 leaves nothing for variance. Allocate 60-80% of the SLO to the typical path; reserve the rest for surprises.

7. **No coverage SLI.** Your service returns a 200 happily while silently falling back to a heuristic because the feature pipeline is broken. Without a coverage metric you don't know.

8. **"Real-time" without a definition.** Real-time means a number. Specify whether you mean tens of milliseconds (online), seconds (streaming), or "fresher than yesterday" (frequent batch). Pinning the number changes the architecture.

9. **One feature pipeline, two implementations.** Training reads from a Spark job. Serving reads from a Go service. Both are derived from "the same logic." Both diverge inside three months. See [Module 02](../02-feature-stores/README.md).

10. **No error-budget policy.** SLOs without consequences do not change behaviour. Write down the action that follows a breach.
