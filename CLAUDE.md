# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Tetris in vanilla JavaScript + HTML5 Canvas. No dependencies, no build step, no tests, no linter. The README is in Spanish; keep docs/UI text consistent with it.

## Running

Open `index.html` directly, or serve statically (`python -m http.server 8000`, then http://localhost:8000).

## Architecture

Three files, all logic in `game.js` (~300 lines, global scope, no modules):

- `index.html` — `#board` canvas (300×600), `#next-canvas` preview, HUD elements (`score`, `lines`, `level`), and an `#overlay` used for both pause and game over.
- `game.js` — constants (`COLS`/`ROWS`/`BLOCK`, `COLORS`, `PIECES`, `LINE_SCORES`), DOM lookups, then module-level mutable state (`board`, `current`, `next`, `score`, ...) declared in one `let` line and reset in `init()`. Game flow: `init()` → `requestAnimationFrame(loop)`; `loop` accumulates time into `dropAccum` against `dropInterval` (shrinks with `level`) and calls `softDrop`/`lockPiece` → `merge` → `clearLines` → `spawn`. `draw()`/`drawNext()` re-render each frame; `ghostY()` computes the ghost piece. Rotation (`rotateCW` + `tryRotate`) uses basic wall kicks; `collide(shape, ox, oy)` is the shared collision check.
- `style.css` — layout and theming.

Piece colors are indexes into `COLORS`; board cells store the color index (0 = empty).
