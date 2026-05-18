# VS Code extensions for this course

A short, opinionated list. Skip anything that doesn't fit your workflow.

| Extension | Why |
|-----------|-----|
| **Markdown All in One** (yzhang.markdown-all-in-one) | Outline, TOC generation, list continuation. Without this, editing the longer module READMEs is painful. |
| **Markdown Preview Mermaid Support** (bierner.markdown-mermaid) | Renders Mermaid blocks in VS Code's preview pane. GitHub already renders them natively, but local preview saves round-trips. |
| **GitHub Pull Requests and Issues** (github.vscode-pull-request-github) | Review and triage PRs inline if you fork this course or push your own additions. |
| **Excalidraw** (pomdtr.excalidraw-editor) | Diagrams beyond what Mermaid handles cleanly. Save as `.excalidraw.png` (with embedded scene) in `diagrams-shared/` if you need this. The course does not currently use any Excalidraw figures — Mermaid is enough. |
| **Jupyter** (ms-toolsai.jupyter) | Required for the `calculators/` notebooks. |
| **Python** (ms-python.python) | Required for the notebooks. |
| **Code Spell Checker** (streetsidesoftware.code-spell-checker) | Catches typos in long Markdown. The repo's recommended ignore-words list lives at `.cspell.json` if you create one. |
| **Better Comments** (aaron-bond.better-comments) | If you take inline notes in the Markdown. |

## How to install

VS Code command palette > "Extensions: Install Extensions" > paste each ID.

## Keyboard shortcuts worth knowing

- `Ctrl+Shift+V` — open Markdown preview side-by-side.
- `Ctrl+Shift+P` > "Markdown: Create Table of Contents" — auto-insert a TOC.
- `Alt+Shift+F` — format the file (use with Prettier if installed).

## What's NOT recommended

- Copilot-style autocomplete. The course is reading and thinking, not generation. Disable it while you read so your eyes stay on the prose.
- Any "AI summarize this page" extension. The cheat sheets already do that.
