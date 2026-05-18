# 08 — Search pitfalls

1. **Pure neural retrieval.** Loses on rare-term and OOD queries. Hybrid wins.

2. **Skipping the reranker.** The biggest single quality lever; routinely under-bought.

3. **Query understanding as a single sequential pipeline.** Spell -> intent -> NER -> expansion in series. Parallelize independent stages.

4. **Position bias in training labels.** Click logs from position 1 are 10x the click rate; the model learns "show whatever's at position 1." Correct it.

5. **Heavy personalisation without diversity floor.** Each user sees a narrower slice; long-tail content starves.

6. **No zero-result fallback.** Empty page is a worse UX than imperfect results.

7. **Indexing pipeline that doesn't keep up.** Index lag of hours = product feels broken.

8. **Synonym sprawl.** Manual synonym lists grow without curation; conflicts accumulate.

9. **Per-tenant index in a shared cluster** with no rate limiting. One noisy tenant breaks others.

10. **Query rewriting with no A/B safety.** A new rewriter ships to all queries; rare queries break silently. Slice rollout.
