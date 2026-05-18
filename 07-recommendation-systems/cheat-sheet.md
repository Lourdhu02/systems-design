# 07 — Recommendation Systems cheat sheet

1. **Two stages.** Retrieval (recall@1000, cheap) -> Ranking (NDCG@K, heavy). Post-processing for diversity and rules.

2. **Two-tower retrieval.** User tower computed online; item embeddings precomputed and ANN-indexed.

3. **In-batch negatives + log-q correction** for two-tower training. Without log-q, the model under-recommends popular items.

4. **Hard negative mining** sharpens retrieval more than any other trick.

5. **Pairwise LTR loss** is the production sweet spot. Listwise rarely worth the complexity.

6. **Multiple retrievers in parallel.** Two-tower + similar-to-recent + trending + cold-start. >50% of clicks come from items surfaced by only one retriever.

7. **Session signals are the highest-leverage feature additions in modern recsys**, not daily-batch features.

8. **Cold start has three flavours.** Item: content embedding. User: bootstrap + bandit. Both: hard, use exploration.

9. **Exploration matters.** 1-5% of impressions to a bandit. Closes the feedback loop; prevents model collapse.

10. **Diversity is not optional.** MMR, DPP, or per-category caps. Short-term clicks dip; long-term engagement rises.
