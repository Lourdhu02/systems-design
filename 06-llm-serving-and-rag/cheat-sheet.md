# 06 — LLM Serving and RAG cheat sheet

1. **Two phases:** prefill (compute-bound, parallel) and decode (memory-bandwidth-bound, sequential). Different bottlenecks.

2. **Continuous batching is mandatory.** New requests join the in-flight batch every decode step. 5-20x throughput uplift over dynamic batching.

3. **PagedAttention** (vLLM, 2023). KV cache as virtual memory; 2-4x more concurrent requests.

4. **Prompt caching is the dominant cost lever.** Cached reads ~10% the price of fresh input tokens. Maximise stable prefix length.

5. **Output tokens are 5x more expensive than input.** Constrain output length aggressively.

6. **Speculative decoding** (Leviathan 2023, Medusa 2024) gives 1.5-3x decode speedup.

7. **RAG seven decisions:** chunk size, embedding model, index type, hybrid retrieval, reranker, context packing, eval harness. Reranker and chunking are the most under-bought.

8. **Hybrid retrieval (BM25 + vector + reranker) beats pure vector** in production. Always.

9. **Lost-in-the-middle:** put most-relevant chunk first, second-most-relevant last, rest in middle. Or just cut aggressively.

10. **Evals are infrastructure**, not a notebook. Golden set + model-graded + pairwise, run on every PR.

11. **Token economics:** LLM inference is 85-95% of a RAG bill. Five levers: cache hit, output length, model routing, smaller model, less retrieved context.

12. **Agents: keep them shallow.** 3-5 tool calls deep is most production agents. Long loops compound errors.
