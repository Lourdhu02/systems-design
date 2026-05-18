# 09 — Real-time ML cheat sheet

1. **"Real-time" is a latency band.** Pin a number before designing.

2. **Streaming features ≠ online learning.** Most "real-time ML" needs are streaming features + frequent batch retraining, not online weight updates.

3. **Event time, not processing time.** Late events lie about ordering otherwise.

4. **At-least-once + idempotent aggregations** is the right trade-off for most ML features. Exactly-once is overkill.

5. **Online learning loses in production** for almost every consumer recsys / fraud / ad use case. Adversarial poisoning + no rollback = pain.

6. **Late labels are the hardest part of fraud / marketing / loan ML.** Use proxy labels + label-buffered training.

7. **Fraud is adversarial.** Retrain frequently; ensemble; keep some signals hidden from the model so they can't be optimised against.

8. **Backfills via Kafka replay** for short outages; recompute from lakehouse for long ones. Set retention accordingly.

9. **A new streaming feature requires replay** to produce a historical timeline for training.

10. **Anomaly detection is staged.** Cheap statistical filter -> mid-tier ML -> human review. Don't try to score everything heavily.
