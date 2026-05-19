# Contributing

> This repo is intentionally opinionated. Good contributions preserve that: dated claims, explicit trade-offs, real production failure modes, and sources you can trace.

If you are contributing through Codex or another coding agent, pair this file with [`AGENTS.md`](./AGENTS.md).

---

## What belongs here

Useful contributions usually fall into one of these buckets:

- Fix a factual error, stale claim, broken link, or misleading diagram.
- Add a citation for a claim that currently lacks one.
- Improve a module's `README.md`, `cheat-sheet.md`, `exercises.md`, `pitfalls.md`, or `case-studies/`.
- Add a calculator in `calculators/` when the trade-off is easier to understand numerically.
- Tighten language, tables, or cross-links without changing meaning.

What does **not** belong here:

- Undated vendor claims presented as if they are current forever.
- Interview-prep fluff.
- Uncited numbers.
- Large code samples inside lesson files.
- Marketing language for a tool or vendor.

---

## First principles

Before you change anything, align to these repo rules:

| Topic | Rule |
|------|------|
| Quantitative claims | Cite a source and year. No invented statistics. |
| Vendor architectures | Treat them as snapshots in time. Date them. |
| Writing style | Prefer simple language, tables, and diagrams over dense prose. |
| Engineering lens | Emphasise trade-offs, ops, cost, and failure modes. |
| Lessons | No runnable source code inside lesson files. Pseudocode is fine. |
| Diagrams | Use Mermaid so GitHub renders them. |
| Jargon | Cross-link to [`glossary.md`](./glossary.md) when useful. |

If a claim is older than two years, assume the underlying production system may have changed and write accordingly.

---

## Repo shape

The numbered modules are intentionally consistent:

```text
00-foundations/ ... 14-case-studies-deep-dives/
  README.md
  cheat-sheet.md
  exercises.md
  pitfalls.md
  case-studies/
```

When you add module content, match the existing file names and tone.

File roles:

| File | Purpose |
|------|---------|
| `README.md` | Main lesson. Teach the concept, architecture, and trade-offs. |
| `cheat-sheet.md` | Dense one-page reference. Prefer tables. |
| `exercises.md` | Design questions with solutions under `<details>`. |
| `pitfalls.md` | Common production mistakes and how to avoid them. |
| `case-studies/` | Real systems, dated and sourced. |

---

## Content standards

### Lessons

Use the established pattern where possible:

1. Start with a time budget and "By the end you can".
2. Introduce the system shape with a diagram or table early.
3. Explain the core trade-offs.
4. Include failure modes, operational realities, or cost implications.
5. End with cross-links and sources.

### Case studies

A good case study usually includes:

1. The problem the team was solving.
2. A Mermaid architecture diagram.
3. The two or three load-bearing design decisions.
4. What they got wrong, rebuilt, or had to work around.
5. What a reader should steal for their own system.
6. Primary sources.

### Exercises

- Prefer design questions over trivia.
- Make the prompt specific enough that a real engineer could answer it.
- Put the worked solution under `<details>`.
- Explain trade-offs, not just the final answer.

### Pitfalls

- Focus on mistakes people actually make in production.
- Name the failure mode clearly.
- Include the mitigation or design discipline that prevents it.

---

## Citations and freshness

This repo is only useful if readers can trust where claims came from.

- Put the year next to vendor posts, papers, and books.
- Prefer primary sources: papers, engineering blogs, official docs, talks.
- If two sources disagree, either resolve it or say that they disagree.
- If you are inferring beyond the source, make that explicit.
- Do not present a 2019 architecture as if it is still the 2026 architecture.

For vendor stacks, phrasing like this is preferred:

> "This is what the team described publicly in 2023; the current production system may differ."

---

## Formatting conventions

- Prefer Markdown tables when comparing options.
- Keep headings short and scannable.
- Use relative links inside the repo.
- Keep Mermaid diagrams readable in raw text, not just rendered form.
- Use fenced code blocks only for pseudocode, formulas, JSON examples, or calculator-friendly snippets.
- Keep notebooks in `calculators/`; keep prose in Markdown.

---

## A clean contribution workflow

1. Pick one coherent improvement.
2. Read the surrounding module so your change fits the local voice.
3. Make the smallest change that meaningfully improves accuracy, clarity, or usefulness.
4. Check that links still resolve and Mermaid blocks are balanced.
5. Re-read for unsupported claims and overstatement.

Small, sharp pull requests are easier to review than broad rewrites.

---

## Pull request checklist

Before you open a PR, verify:

- The change matches the repo's production-systems lens.
- Every quantitative claim has a source and a year.
- New vendor claims are dated.
- New jargon is explained or linked to the glossary.
- New diagrams use Mermaid and are readable on GitHub.
- New links are relative and not broken.
- Lesson files do not contain runnable source code.
- The change improves signal, not just word count.

---

## Good starter contributions

If you want a safe place to start:

- Tighten a table that is doing too much work in prose.
- Add a missing citation.
- Add a cross-link between related modules.
- Fix a stale year or overconfident vendor claim.
- Add one well-scoped exercise or one concrete pitfall.

That is enough. This repo compounds through many small, accurate improvements.
