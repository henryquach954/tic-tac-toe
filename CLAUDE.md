# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file browser game (`tictactoe.html`). No build system, no dependencies, no package manager. Open the file directly in a browser to run it.

```bash
open tictactoe.html
```

## Git workflow

Commit and push to GitHub (`henryquach954/tic-tac-toe`) after every meaningful unit of work — a feature added, a bug fixed, a refactor completed. Never batch multiple unrelated changes into one commit. The goal is that the repo always reflects the latest working state so no progress is ever lost.

```bash
git add <files>
git commit -m "short imperative summary"
git push
```

Commit message rules:
- Start with an imperative verb: "Add", "Fix", "Update", "Remove"
- Describe *why* the change was made, not just *what* changed
- Keep the subject line under 72 characters
- Always append the co-author trailer:
  `Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>`

## Architecture

Everything lives in `tictactoe.html` as a single file with three sections:

- **`<style>`** — all CSS; dark theme using a `#1a1a2e` / `#16213e` / `#0f3460` palette, accent `#e94560` (X/red) and `#a8dadc` (O/blue)
- **`<body>`** — static markup; the 9 board cells are `<div class="cell" data-i="N">` where `N` is the flat index 0–8
- **`<script>`** — all game logic; no framework

### Game logic

- `board` — 9-element array of `''`, `'X'`, or `'O'`
- `WINS` — hardcoded array of all 8 winning index triples
- `checkWinner()` — iterates `WINS`; returns `{ winner, line }` on a win, `{ winner: null, line: [] }` on a full board draw, or `null` if the game is still in progress
- `init()` — resets board, clears cell classes/text, resets `current` to `'X'`
- Score state (`scores.X`, `scores.O`, `scores.D`) persists across `init()` calls (i.e., across games within the same page session)
- Cell state is tracked via both the `board` array and CSS classes: `.taken` blocks re-clicks, `.x`/`.o` set color, `.win` highlights the winning triple
