# Spotify — Voyager (2023)

## Problem

Spotify recommends songs, podcasts, and playlists across hundreds of millions of users. Their internal ANN library, originally based on Annoy (2013), was the workhorse for a decade but had limitations as embedding dimensions grew and query throughput requirements increased. Spotify built **Voyager** (2023) as the next-generation ANN library, optimised for the same workloads but using HNSW under the hood.

## Architecture (conceptual)

```mermaid
flowchart LR
    EMB[Embedding models<br/>user, track, podcast, playlist] --> BUILD[Voyager index build]
    BUILD --> IDX[(HNSW index<br/>memory-mapped file)]
    IDX --> SVC[Recommendation services]
    SVC --> ANN[ANN query]
    ANN --> CAND[Candidates -> ranker]
```

Voyager exposes a Python and Java API; the index is a single memory-mapped file that can be embedded in any process. No separate vector-DB service.

## Three load-bearing decisions

1. **HNSW over Annoy's tree-based approach.** Better recall-latency trade-off at modern embedding dimensions (256-1024).
2. **Embedded library, not standalone service.** Spotify's serving stack already has process-level ML inference; an ANN library that runs in-process avoids a network hop.
3. **Memory-mapped index file.** Multiple processes share the same in-memory index without duplication; OS handles paging.

## What didn't fit

- **Updates.** Annoy is build-once-read-many; Voyager inherits the same constraint (HNSW supports inserts but expensive ones). Spotify's pattern is to rebuild offline and ship the new index to serving replicas.
- **Cross-language consistency.** Java and Python clients need to agree on float layout, hash functions, etc. Spotify invested in a shared core to avoid drift.
- **Hybrid retrieval.** Voyager is vector-only. Hybrid pipelines compose Voyager with a separate lexical retriever.

## What you should steal

- The **embedded-library** pattern. If your serving stack is already ML-heavy, an in-process ANN library beats a microservice on latency and operational surface.
- The discipline of **build-offline / serve-online**. Rebuilding daily / weekly and shipping a new index file is far simpler than online inserts.
- **Memory-mapped indexes** for multi-tenant or multi-process serving. Free deduplication.

## Sources

- "Introducing Voyager: Spotify's Open Source Approximate Nearest Neighbor Library" (Spotify Engineering, 2023).
- Voyager GitHub repository, current release.
- "From Annoy to Voyager: A Decade of ANN at Spotify" (Spotify Engineering, 2023).
