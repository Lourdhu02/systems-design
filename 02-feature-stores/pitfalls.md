# 02 — Feature Stores pitfalls

1. **Computing features twice — once in SQL for training and once in service code for serving.** The single most common skew bug. Use one declaration, two backends.

2. **Naive `GROUP BY user_id` for training joins.** Leaks future information into past rows. PIT join always.

3. **No `epsilon` in the PIT join.** Using `event_time < T_row` is generous to the model because the online store at T_row is some seconds stale. Use `T_row - epsilon`.

4. **Streaming feature with no offline parity.** Online serving works; training can't backfill. Always materialize the streaming feature to an offline sink.

5. **Hot keys ignored.** One viral product becomes 10x the QPS of all others combined. Without an in-replica cache, your online store goes hot-shard.

6. **30 sequential feature reads per request.** Replace with one multi-get.

7. **No fallback on feature-fetch failure.** Throwing 500 because Redis blinked is worse than returning a default with a `feature_fallback_count` metric.

8. **Feature ownership black hole.** A feature with no owner is a feature with no SLO and no deprecation path. Make ownership a required column at registration time.

9. **Features that capture leakage via row order.** A feature defined as "last value in the table" in SQL silently joins against the future when the table is unsorted.

10. **Skipping logged-feature training.** Teams insist on training from the warehouse "for clean data." The serving-logged training set is the only one that exactly matches what production sees.
