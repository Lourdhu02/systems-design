# 06 — LLM Serving and RAG exercises

---

### 1. INTERVIEW. Design a customer-support assistant: 5M monthly queries over a 500k-document knowledge base. Latency SLO p95 = 5 s.

<details><summary>Solution</summary>

Architecture: see [`../diagrams-shared/rag-reference-architecture.md`](../diagrams-shared/rag-reference-architecture.md).

Sizing:
- 5M monthly = ~2 QPS average, ~10 QPS peak. Small from a serving perspective.
- 500k documents at ~10 chunks per doc = 5M chunks. HNSW in RAM fits in one r6i.8xlarge.
- Cross-encoder reranker on a single L4: more than enough capacity.
- LLM: hosted (Claude / GPT / Gemini class) at this volume; self-host is uneconomical at <100 QPS.

Decisions:
1. Chunking: semantic split on headings, 800-token chunks, 15% overlap.
2. Embedding: hosted (OpenAI / Cohere) initially; reassess at 5M docs.
3. Retrieval: BM25 (OpenSearch) + vector (OpenSearch k-NN) + RRF.
4. Rerank: bge-reranker-large on L4.
5. Context packing: top-8 reranked chunks, ~6k tokens of context.
6. Eval: 500-case golden set, weekly pairwise, model-graded for open-ended.
7. Prompt cache: pin system prompt + tool definitions at top. Expect ~50% hit rate after warmup.

SLOs: p95 latency 5 s, NDCG@10 on golden >= 0.85, hallucination rate <1% on cited-answer subset.
</details>

---

### 2. DECISION. Your bill is $80k/month, 90% of which is LLM tokens. Cut it in half.

<details><summary>Solution</summary>

Order of levers (highest expected ROI first):

1. **Prompt cache hit ratio.** Re-architect prompts so the system prompt, tool definitions, and (for RAG) the most-popular context lives at the front. Going from 30% cache hits to 70% on the input tokens typically halves the input bill, which is ~half the LLM bill on a typical workload.
2. **Output length cap.** Many systems generate 500-1000 output tokens when 150-200 would suffice. Constrain with a strict max + a tight system prompt. Easy 30-50% savings on output tokens.
3. **Model routing.** A small classifier sends easy queries (FAQ-shaped, single-doc-retrievable) to a 7B model and complex ones to the frontier model. Saves 50-70% on the easy slice.
4. **Less retrieved context.** Halving the retrieved context halves the input bill on uncached prompts.
5. **Self-host the cheap model.** Only worth it past ~$50-100k/month sustained.

Combining (1) and (2) typically gets you the 50% target. (3) is more invasive but durable.
</details>

---

### 3. INTERVIEW. Explain prefill vs decode and why a 4k-token prompt + 200-token output costs less than a 200-token prompt + 4k-token output.

<details><summary>Solution</summary>

Prefill: parallel attention over the prompt; the model processes all prompt tokens in essentially one forward pass. Throughput is GPU-limited but very high.

Decode: autoregressive; one token at a time, each step rereading the KV cache. Memory-bandwidth-limited; the GPU is largely idle.

A 4k prompt + 200 output:
- Prefill cost ~ proportional to 4k tokens of compute, but compute is cheap per token in prefill.
- Decode cost ~ proportional to 200 tokens, each requiring a full forward pass with the growing KV cache.

A 200 prompt + 4k output:
- Prefill cost ~ trivial.
- Decode cost ~ 4k forward passes, each more expensive than the last because the KV cache grows.

Net: decode tokens are 5-10x more expensive than prefill tokens at the API price level (matches Anthropic / OpenAI pricing). Long outputs are far costlier than long inputs.
</details>

---

### 4. CASE-STUDY READ. Read the vLLM paper (Kwon et al., SOSP 2023). What is the one design choice that breaks the previous serving status quo?

<details><summary>Solution</summary>

**Treating the KV cache as virtual memory** with fixed-size pages and per-request page tables.

Previous LLM servers allocated a contiguous block of KV cache per request, sized for the max sequence length the request might use. With variable-length conversations, most of that block is unused for most requests; the GPU's KV-cache space gets fragmented and underutilised. At high concurrency this is the limiter.

PagedAttention's insight: pages are allocated on demand, freed when sequences finish or are evicted, and shared between requests when prefixes match (the basis of self-hosted prompt caching). The result is 2-4x more concurrent requests on the same GPU and a clean mechanism for prefix sharing.

The conceptual ancestor is OS virtual memory — same trick, different domain. Once you see it, you can't unsee it.
</details>

---

### 5. DECISION. The eval team says your RAG system regressed by 8% on the golden set after a prompt change, but a sample of production logs looks fine. What do you do?

<details><summary>Solution</summary>

The golden set is small and the prompt change might have shifted behaviour on a slice the golden set covers more than production. The diagnostic playbook:

1. **Inspect the regressed cases.** Look at the specific examples in the golden set that flipped. Are they representative of production? Sometimes golden sets over-index on edge cases.
2. **Run a pairwise eval on production-sampled queries.** 100-300 randomly sampled production queries; compare old vs new. If new wins or ties on production, golden set is misleading.
3. **Compute a metric per query category.** The regression might be concentrated in one slice (e.g., "questions about feature X") and you have a story.
4. **Decide.** If golden set regressed on a critical slice that production traffic underrepresents but customers expect to work, keep the old prompt. If golden set regressed on a slice that's not representative, ship the new one but enlarge the golden set in the regressed area.

The deeper lesson: **golden sets need to evolve with the product**. A static golden set becomes a museum.
</details>

---

### 6. INTERVIEW. Spec the prompt cache strategy for a customer-support agent that uses tools.

<details><summary>Solution</summary>

```text
Cache prefix (frozen across requests):
  - System prompt + persona
  - Tool definitions
  - Few-shot examples
  - Approximate position: tokens 0..N (e.g., N = 4000)

Cache middle (cached when popular):
  - Retrieved context from the knowledge base
  - The same article appearing in many requests can be cached
  - Hash by chunk_id; cache hit detected by exact-prefix match

Variable suffix:
  - User message
  - Conversation history (for multi-turn)
```

Hosted APIs detect the longest common prefix; you maximise cache hits by **structuring prompts so the variable part is always at the end**. Don't put a user's name halfway through the system prompt.

Measurement: instrument cache-hit-token ratio per request; aim for >70% on a mature deployment. Below 50% means your prompt structure is leaking variability into the cached prefix.

TTL: hosted caches typically have 5-minute TTLs (Anthropic) or similar. For high-volume systems this is fine; for sparse traffic, evaluate whether longer self-hosted caching is worth it.
</details>

---

### 7. DECISION. The team proposes "RAG with a 200k-token context, just retrieve more and let the model figure it out." Argue against.

<details><summary>Solution</summary>

Three problems:

1. **Cost.** 200k input tokens per query at $3/M is $0.60 per query. At 1M queries/day that's $600k/month just on input tokens.
2. **Lost-in-the-middle** (Liu et al., 2023). LLMs underweight the middle of long contexts. Performance peaks around 8-16k of focused context and degrades from there.
3. **Latency.** Prefill is parallel but not free; 200k token prefill is meaningfully slower than 8k. p99 can blow the SLO.

Better: rerank to the top 8-16 chunks (~10k tokens of context), let the model think. Quality typically goes UP, cost goes down by ~20x, latency drops.

The exception: research-style "summarise this 200-page document" workflows where the whole document is the input. There you need long context. RAG isn't that workflow.
</details>

---

### 8. INTERVIEW. Walk through how speculative decoding works and where it breaks down.

<details><summary>Solution</summary>

A small "draft" model proposes the next K tokens cheaply. The big model verifies all K in a single forward pass (cheap because forward passes are parallel). For each position, accept the draft's token if it matches the big model's top choice (under sampling, accept proportional to the ratio of probabilities).

Throughput uplift: 1.5-3x on typical tasks. Comes from amortising K decode steps into one big-model forward.

Where it breaks down:

1. **Draft quality.** If the draft model is wrong most of the time, the verification accepts nothing and you've added overhead.
2. **Domain mismatch.** A draft model trained on general data may be poor on code or math, where the big model wins. Acceptance rate plummets.
3. **High batch sizes.** When the big model is already saturating its compute, the verification step takes longer than a single decode. Less benefit.

Variants: Medusa (Cai et al., 2024) bakes draft heads into the big model; EAGLE and similar methods adjust the draft strategy adaptively.
</details>

---

### 9. CASE-STUDY READ. Read the Orca paper (Yu et al., OSDI 2022). What was the previous status quo for LLM batching, and what did Orca change?

<details><summary>Solution</summary>

Previous status quo: **request-level batching**. The server collected N requests, ran prefill + all decode steps for the batch together, and only after every request in the batch finished did the next batch begin.

Two problems:

1. **Tail latency dominates.** The longest output in the batch sets the latency for the whole batch. Short responses wait for long ones.
2. **GPU idle time.** As requests in the batch finish at different times, the active batch shrinks; the GPU runs underutilised toward the end.

Orca's change: **iteration-level batching** (later popularised as continuous batching). Every decode iteration is a fresh decision. Finished requests leave the batch immediately; queued requests join immediately. Tail latency decouples from batch composition.

This is the architectural ancestor of vLLM's continuous batching. The throughput uplift is 5-20x depending on workload mix.
</details>

---

### 10. INTERVIEW. Design an eval harness for an internal RAG product. What goes in CI vs nightly vs weekly?

<details><summary>Solution</summary>

```text
CI (every PR, < 5 min):
  - Golden set of 100-200 cases, exact-match or simple grading.
  - Latency budget check (p95 within budget on the eval set).
  - Block PR on regression > 2 percentage points without sign-off.

Nightly (full eval set, ~30 min):
  - Full golden set of 1000+ cases.
  - Model-graded eval on open-ended answers (judge: a strong model).
  - Per-category breakdown (FAQ, technical, account, billing).
  - Dashboard with trends and per-PR contribution.

Weekly (~1 hour):
  - Pairwise eval against the current production candidate.
  - Production-sampled fresh queries (with consent) to refresh the eval set.
  - Red-team prompt sweep (jailbreak attempts, PII leakage).

Monthly:
  - Calibration: human-grade a random sample to check that model-graded scores still correlate with human judgment.
  - Eval set audit: remove cases that have become trivial; add cases for new product areas.
```

Storage: eval cases live in a git-tracked YAML / JSONL with owner and date columns. Scoreboards rendered to a dashboard. Treat eval set like production code, not like a notebook.
</details>

---

### 11. DECISION. The product team wants to add an agent that can run shell commands. What do you require before shipping?

<details><summary>Solution</summary>

Hard requirements:

1. **Hermetic sandbox.** Firecracker, gVisor, or a Docker container with strict seccomp profile. Treat all tool inputs and outputs as untrusted.
2. **No network egress by default.** Whitelisted egress only.
3. **No persistent state.** Each agent session gets a fresh sandbox.
4. **Strict tool-call budget.** Cap retries and total calls per session.
5. **Timeout per call.** Tool calls without timeouts hang the agent.
6. **Logging of every tool call** with input, output, agent decision, and outcome.
7. **Auth and authorization.** Which user authorised which agent run. If the agent fails midway, what gets rolled back?
8. **Red-team plan.** Prompt-injection attempts via tool outputs (a fetched web page contains "ignore previous instructions") must be tested.

Soft requirements:
- User confirmation for destructive actions.
- A dry-run mode that returns "I would have done X" without doing it.
- Clear UI affordances showing what the agent is doing in real time.

Without these, do not ship.
</details>

---

### 12. INTERVIEW. Compare hosted LLM APIs to self-hosted vLLM for a 100 QPS product. What's the break-even?

<details><summary>Solution</summary>

Hosted (e.g., Claude Sonnet, GPT-4o-class) at typical 2025-2026 pricing of ~$3/M input + $15/M output, with 50% cache hit on input:

- Assume 2k input + 300 output per query.
- 100 QPS = 100 x 86400 = 8.6M queries/day.
- Input tokens: 8.6M x 2k x (0.5 x 1 + 0.5 x 0.1) = 9.5B tokens/day x $3/M = $28k/day.
- Output: 8.6M x 300 = 2.6B x $15/M = $39k/day.
- Total: ~$67k/day = ~$2M/month.

Self-hosted on H100 cluster:
- vLLM on a frontier-quality 70B model: roughly 200-400 tokens/sec/H100 sustained for decode.
- 2.6B output tokens/day = ~30k tokens/sec sustained. 100-200 H100s.
- At $3/H100-hour on demand: 100 x 24 x $3 x 30 = $216k/month. Plus engineering ops.

Self-hosted is roughly 10x cheaper in steady state, but only if:
- Traffic is steady (spikes mean over-provisioning).
- Your team can operate a vLLM cluster (non-trivial).
- You can accept slightly lower model quality (open-weight 70B is good but not API-frontier).

Break-even: roughly $200-500k/month spend, depending on quality / ops tolerance. Below that, hosted is cheaper end-to-end. Above that, the math turns over.
</details>
