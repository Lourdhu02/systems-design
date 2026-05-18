# 14 — Case Studies (deep dives)

> Time budget: 180 minutes total — about 30 minutes per case study. Read these after the topical modules; they're load-bearing for the capstone.

**By the end you can:**

1. Trace the evolution of six canonical production ML stacks.
2. Identify the load-bearing decisions and the things each team had to rebuild.
3. Reuse the patterns in your own designs without copying the surface.

---

## Six deep dives

Each case study is a separate file in [`./`](./). All six follow the same template: problem, architecture (with a Mermaid diagram), three load-bearing decisions, what they got wrong, what you should steal, sources.

| # | Case study | Why read it |
|---|------------|-------------|
| 1 | [Uber Michelangelo](./uber-michelangelo-deep-dive.md) | The canonical end-to-end ML platform. Sets the vocabulary for everything else. |
| 2 | [Netflix recommendations stack](./netflix-recommendations-deep-dive.md) | Personalisation at consumer-internet scale; multi-objective ranking. |
| 3 | [Spotify Discover Weekly](./spotify-discover-weekly-deep-dive.md) | Recsys for music; how a flagship product works under the hood. |
| 4 | [Pinterest PinSage](./pinterest-pinsage-deep-dive.md) | Graph-based embeddings; cold-start at scale. |
| 5 | [DoorDash search and ranking](./doordash-search-ranking-deep-dive.md) | Marketplace ranking; ETA model integration. |
| 6 | [Anthropic / OpenAI inference infra](./anthropic-openai-inference-infra-deep-dive.md) | What is publicly known about LLM serving at frontier scale. |

---

## What pattern repeats across all six

Read the deep dives, then come back here. The patterns that show up in every single one:

1. **Logged features become training data.** Stripe, Uber, Pinterest, DoorDash, Netflix, Spotify — every mature production team converges on this.
2. **One feature definition, two backends.** Offline and online compute paths derived from a single source.
3. **Two-stage architecture for retrieval and ranking** at consumer scale. Retrieval narrows, ranker scores precisely.
4. **Streaming features are the highest-leverage feature additions** in modern recsys / search.
5. **Eval / monitoring as infrastructure, not notebooks.** Quality SLO, golden sets, regression gates.
6. **Per-cohort metrics** because aggregate hides the bias.
7. **Model registry + canary + shadow** as the deployment shape.
8. **Logging predictions with model version + features used + request id** as the audit trail.

If you find a team that doesn't do these things, they will be doing them in 18 months or they will be having a bad time.

---

## Cross-links

- Each deep dive links to the relevant topical module.
- Up next: [15 Capstone](../15-capstone/README.md).
