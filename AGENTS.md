# AGENTS.md

Instructions for Codex and similar coding agents working in this repository.

This repo is a documentation-first course on production ML / AI systems design. Contribute like an editor with engineering judgment, not like a generic code generator.

---

## Mission

- Improve accuracy, clarity, or usefulness.
- Preserve the repo's production-systems lens.
- Prefer small, reviewable changes over broad rewrites.

If a requested change conflicts with the repo's house rules, say so and choose the narrowest compliant path.

---

## Non-negotiable rules

- Do not invent statistics, benchmarks, or vendor capabilities.
- Every quantitative claim must have a source and a year in the prose.
- Treat vendor architectures as dated snapshots, not timeless truths.
- Do not add runnable source code to lesson files. Pseudocode is fine; runnable artifacts belong in `calculators/`.
- Use Mermaid for architecture diagrams so GitHub renders them.
- Prefer relative repo links.

If a claim is old, write with explicit time-bounding language such as "as described publicly in 2023."

---

## Repository shape

The numbered modules are intentionally consistent. Preserve that structure.

```text
00-foundations/ ... 14-case-studies-deep-dives/
  README.md
  cheat-sheet.md
  exercises.md
  pitfalls.md
  case-studies/
```

File intent:

- `README.md`: main lesson with system shape, trade-offs, failure modes, and sources.
- `cheat-sheet.md`: dense quick reference, usually table-heavy.
- `exercises.md`: design questions with worked solutions under `<details>`.
- `pitfalls.md`: real production mistakes and mitigations.
- `case-studies/`: real systems, explicitly dated and sourced.

---

## How to work

Before editing:

1. Read the surrounding file and at least one adjacent file in the same module when relevant.
2. Match the local tone and formatting.
3. Prefer the smallest useful change.

When editing:

- Keep prose direct and concrete.
- Favor tables, lists, and diagrams over long narrative blocks.
- Add or tighten cross-links when they genuinely help navigation.
- Link jargon to [`glossary.md`](./glossary.md) when useful.
- Use absolute dates when freshness matters or when relative wording would be ambiguous.

After editing:

- Re-read for unsupported claims or overstatement.
- Check that Markdown links still resolve.
- Check Mermaid blocks for obvious syntax mistakes and balanced fences.

---

## What good contributions look like

Good:

- Fixing a stale or overconfident claim.
- Adding a missing citation.
- Tightening a trade-off table.
- Adding one solid exercise or one concrete pitfall.
- Improving a diagram without changing its meaning.

Bad:

- Expanding word count without adding signal.
- Turning lessons into interview prep.
- Adding vendor marketing language.
- Rewriting many modules at once without a specific reason.
- Smuggling in speculative claims as fact.

---

## Git and review expectations

- Keep branches and PRs focused on one coherent improvement.
- Prefer docs-only changes unless the task clearly belongs in `calculators/`.
- Do not fabricate AI authorship or attribution metadata in commit messages. If a GitHub-connected Codex workflow adds attribution, let the integration handle it.

If you are preparing a PR description or summary, call out:

1. What changed.
2. Why it improves the repo.
3. Any claims that still need citation or verification.

---

## Priority order when in doubt

1. Accuracy
2. Citation quality
3. Clarity
4. Consistency with existing module structure
5. Brevity

When these trade off, choose the higher item.
