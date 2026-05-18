# 09 — Real-time ML pitfalls

1. **"Real-time" with no number.** Wastes architecture on premature optimisation. Pin the latency requirement first.

2. **Online learning by default.** Adversarial poisoning, no rollback, diverging replicas. Use frequent batch.

3. **Processing-time windows.** Late events produce wrong feature values silently.

4. **No event-id dedup.** At-least-once delivery double-counts on retry.

5. **Kafka retention too short.** A 4-hour outage with 2-hour retention means you cannot replay; you're falling back to lakehouse recompute under pressure.

6. **No backfill plan for new features.** Adding a streaming feature with no historical timeline means you can't train on it.

7. **Exactly-once everywhere.** Operational cost for guarantees you didn't need.

8. **Anomaly detector scored against everything.** Expensive model running on 50k events/sec. Stage your filtering.

9. **Late-label denial.** Training the fraud model on labels less than 30 days old, then puzzled why offline metrics overstate online performance.

10. **No alerting on feature freshness.** Pipeline lag is invisible until the product metric tanks.
