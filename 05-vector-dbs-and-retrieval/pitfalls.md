# 05 — Vector DBs pitfalls

1. **Going vector-only.** Production queries have rare tokens; pure vector loses. Always pair with BM25.

2. **Skipping the reranker.** The cheapest, biggest quality lever, repeatedly underbought.

3. **Embedding model swap with no re-embedding plan.** Now your index and your model disagree; recall collapses silently.

4. **Naive filter-after-retrieve.** Top-1000 then filter leaves 3 results when the category is rare.

5. **Synchronous in-place deletes.** Breaks the graph. Use tombstones; reclaim at compaction.

6. **No full-rebuild cadence.** Tombstones accumulate; query latency grows; eventually the index is mostly skipped nodes.

7. **`efSearch` set globally, not by query class.** Some queries can afford 1000; others need 50. Pick per intent.

8. **Cosine distance on un-normalised vectors.** Embeddings drift to non-unit norms; distance metric breaks silently. Normalize.

9. **Index per tenant in a single-process DB.** Sounds clean, ends with 10k indexes and a process that takes hours to start.

10. **No ANN cost telemetry.** Per-query distance-comp counts, page-read counts, and RAM-resident sizes are the only way to debug a regression. Instrument from day one.
