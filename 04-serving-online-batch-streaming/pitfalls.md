# 04 — Serving pitfalls

1. **One request per GPU invocation.** No batching. Throughput is 1-5% of theoretical. Use dynamic batching (Triton) or continuous batching (vLLM).

2. **Synchronous prediction logging.** Your model server's p99 includes the log write. Log async to a Kafka topic.

3. **No canary auto-rollback.** A bad model gets 100% of traffic because the canary criteria are documented in a Notion page nobody reads. Encode the criteria in CI.

4. **Latency budget that doesn't add up.** 200 ms SLO, components sum to 195 ms with no margin. First production incident blows it.

5. **Sequential feature and retrieval calls.** Two independent reads in series instead of in parallel; budget is `feature_ms + retrieval_ms` when it could be `max(...)`.

6. **GPU autoscaling on request rate.** By the time the metric fires, you've missed the spike. Use leading indicators (queue depth, session-start rate).

7. **No warm pool.** Cold start is 60-300 s for the GPU node + 30-180 s for the model. Customers see errors during ramp.

8. **Custom Python server at scale.** No batching, no model versioning, no observability. Hits a ceiling at ~50 QPS.

9. **Streaming inference with no offline replay path.** When the model needs retraining, you can't reconstruct what the streaming pipeline was doing. Always emit to an offline log too.

10. **Edge inference with no fallback.** Old app versions run an old model. Plan the deprecation and the server-side override.
