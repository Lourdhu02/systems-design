# 02 — Feature Stores cheat sheet

1. **One feature definition, two execution backends.** Offline (training) + online (serving) compiled from the same declarative source. This is the only durable train-serve-skew fix.

2. **Point-in-time correctness or your model is broken.** Every feature has an event-time column; every PIT join uses `event_time < T_row - epsilon`. Epsilon matches typical online staleness.

3. **Naive `GROUP BY` joins leak labels.** Trust the feature store's compiler, not your one-off SQL.

4. **Streaming features need offline parity.** Materialise the streaming feature output to an Iceberg sink so training has a historical timeline.

5. **Build vs buy by feature count:** <20 features, just use Redis + SQL. 20-200, adopt Feast / Tecton. >200 or regulated multi-tenant, justify a platform team.

6. **Hot keys are real.** Per-replica in-memory cache for the top keys + an edge cache fronting the online store. YouTube and Pinterest both publish this pattern.

7. **One multi-get, not N gets.** Batch every feature read into one call.

8. **Log the features you actually read at serving time** — that's the training signal. Logged-feature training is the cheapest skew fix that exists.

9. **Always have a fallback value.** When the online store degrades, return a defined default + a metric.

10. **Feature ownership.** Every feature has an owner; every feature has a freshness SLO; every feature has a deprecation policy.
