# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file browser game (`tictactoe.html`). No build system, no dependencies, no package manager. Open the file directly in a browser to run it.

```bash
open tictactoe.html
```

## Git workflow

Every change must be committed and pushed to GitHub (`henryquach954/tic-tac-toe`) after completion. Commit messages should be concise and describe *why*, not just *what*.

```bash
git add <files>
git commit -m "short imperative summary"
git push
```

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
