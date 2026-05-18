# 06 — LLM Serving and RAG pitfalls

1. **Shipping without an eval harness.** Every prompt change is a hope. Within three months you've regressed in production and didn't notice.

2. **Pure vector retrieval.** Production has rare tokens; BM25 + vector + reranker wins.

3. **Naive context filling.** Stuffing 100k tokens of retrieved context. Lost-in-the-middle. Cost. Latency. All bad.

4. **No prompt cache strategy.** Variable strings in the middle of "stable" prompts kill the cache hit ratio.

5. **Output tokens unconstrained.** Default model verbosity is 5-10x more than your product needs. Add a strict cap and a tight system prompt.

6. **Agent loops without budget.** Five tool calls become fifty when the model gets confused.

7. **No tool-call sandbox.** Code execution exposed to prompt injection from fetched web content; the model executes attacker-controlled instructions.

8. **One model for all queries.** Frontier model for the FAQ is paying twice for nothing. Route.

9. **Embedding lock-in not planned for.** When you want to swap models, the cost of re-embedding is a quarter you didn't budget for.

10. **Chunk size set by feel.** "1000 tokens, sounds right" is not engineering. Tune by eval.

11. **Eval set drift not managed.** Golden cases that the model now nails because they're in training data of the next-gen model give false confidence. Audit periodically.

12. **No streaming response.** First-token latency matters even if total latency is the same; users perceive the wait. Stream output back as soon as decode starts.
