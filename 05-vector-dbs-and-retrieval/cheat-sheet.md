# 05 — Vector DBs cheat sheet

1. **HNSW** for hot RAM-resident data. **IVF-PQ** when you need to scale past RAM. **DiskANN** for billion-scale on a single node.

2. **Pure vector rarely wins in production.** Hybrid (BM25 + vector + reranker) is the canonical pipeline.

3. **Cross-encoder reranker over top-100 is the highest-leverage quality lever.** Cheaper than tweaking the embedding model.

4. **Reciprocal Rank Fusion** is the parameter-free default for combining lexical and vector results.

5. **Vector DB pick by what else you operate:** Postgres shop → pgvector. OpenSearch shop → k-NN. Greenfield managed → Pinecone. Self-host at scale → Qdrant / Milvus / Vespa.

6. **Index updates:** lazy (segments + merge), incremental (immediate write), full rebuild (periodic). Real systems do all three.

7. **Deletes use tombstones.** Compaction reclaims space at next full rebuild.

8. **Embedding lock-in is real.** Re-embedding 100M docs costs hours and dollars. Plan before you ingest.

9. **Recall, latency, and cost are a Pareto.** Pick one target metric; measure the others as outcomes.

10. **Cost shape: HNSW = RAM, IVF-PQ = cheap storage + recall hit, DiskANN = SSD I/O.** Pick by your bottleneck.
