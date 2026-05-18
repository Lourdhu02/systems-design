# 08 — Search and Ranking cheat sheet

1. **Default search pipeline:** query understanding -> BM25 + vector retrieval -> RRF fusion -> cross-encoder rerank -> personalisation -> business rules. Same shape as RAG and recsys retrieval.

2. **Query understanding is parallelizable.** Spell, intent, entity, expansion run concurrently in <10 ms total.

3. **BM25 is still the strongest baseline** for keyword-heavy queries; pair it with vector for paraphrase coverage.

4. **Cross-encoder rerank is the single biggest quality lever.** Over top 100-200 candidates, ~+5-15 pp NDCG.

5. **Personalisation layers:** cohort (3-5%), long-term (2-4%), session (1-3%). Diminishing returns; trade-off with diversity.

6. **Indexing pipeline:** real-time event ingest -> enrichment -> segment write -> background merge -> periodic full reindex.

7. **Click data has position bias.** Correct with pairwise loss + position discount, or counterfactual estimators.

8. **Personalisation reduces diversity.** Decide whether your product wants narrow personalisation or broad discovery.

9. **Query rewriting with LLMs** is a 2024-2026 emerging pattern; cost real, lift real, use selectively.

10. **Feed-style "search" is recsys with a query**, not web search. Treat it differently.
