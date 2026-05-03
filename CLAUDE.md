# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file vanilla web game (`index.html`). No build step, no dependencies, no package manager. Open the file directly in a browser to run it.

## Architecture

Everything lives in one HTML file with three co-located sections:

- **CSS** (`<style>`) — dark theme, CSS Grid board, animation keyframes (`pop` on move, `pulse` on winning cells), difficulty badge color overrides per button ID.
- **HTML** (`<body>`) — static shell only; all dynamic text is written by JS. Board cells are identified by `data-index="0–8"`.
- **JS** (`<script>`) — plain global state, no modules or frameworks.

### State

| Variable | Purpose |
|----------|---------|
| `board` | `string\|null[9]` — `'X'`, `'O'`, or `null` |
| `current` | Whose turn: `'X'` or `'O'` |
| `gameOver` | Blocks further input |
| `mode` | `'pvp'` or `'ai'` |
| `difficulty` | `'easy'` \| `'medium'` \| `'hard'` |
| `scores` | `{ X, O, draw }` persists across rounds |

### AI logic (`getAiMove`)

- **Easy** — 85% random; 15% chance it takes an immediate win.
- **Medium** — greedy heuristic: win → block → center → corner → random. Cannot detect forks.
- **Hard** — full Minimax with Alpha-Beta pruning (`minimaxMove` → `minimax`). Unbeatable. AI plays as `'O'`; scores `+10-depth` for AI win, `-10+depth` for player win.

`checkWinner(b)` returns the winning combo array or `null`. `boardWinner(b)` returns the winner symbol or `null` — used inside the recursive minimax to avoid re-destructuring.

## Conventions

- All user-facing text must be in **English** (buttons, status messages, labels) — even if the conversation is in Turkish.
- After every change: `git add <file>` + `git commit` + `git push` to keep `Kozmozs/tic-tac-toe` in sync.
- Commit messages: imperative subject line, bullet body for non-trivial changes.
