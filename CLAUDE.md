# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A themed browser tic-tac-toe game. The entire application is a single self-contained file, `tic-tac-toe.html` (HTML + CSS + vanilla JS, no dependencies, no build step).

## Running

Open `tic-tac-toe.html` directly in a browser. On Windows: `start tic-tac-toe.html`. There is no build, lint, test, or package step — editing the file and reloading the browser is the full dev loop.

## Architecture

Everything lives in the `<script>` block of `tic-tac-toe.html`. Two concerns are deliberately decoupled:

- **Game logic** operates on `state` (a 9-element array of logical marks `''`/`'X'`/`'O'`), `current` (whose turn), and `over`. `getWin()` checks the `wins` table of 8 winning index triples. Status text always uses the logical letters `X`/`O`, so win/draw detection and messaging are theme-independent.

- **Presentation** is driven by the `THEMES` object (keyed by theme id). Each theme defines the background gradient, the displayed symbols for X/O (emoji, e.g. `🎄`/`⛄`), their colors, an `accent`, and a `particle` animation type. `setTheme(id)` swaps the body background, re-renders existing marks via `renderCell()`, and resets the particle system. The logical mark in `state` never changes when the theme changes — only what `renderCell()` draws.

When adding a theme, add an entry to `THEMES`; the picker buttons (`buildThemeButtons()`) and rendering pick it up automatically. A new `particle` value also requires a spawn branch in `spawn()` and a draw branch in `tick()`.

The background animation is a single `requestAnimationFrame` loop (`tick()`) over a fixed full-screen `<canvas id="fx">`. Particles are a flat array of `{x, y, vx, vy, size, kind, ...}`; `kind` (`snow`/`spark`/`glyph`) selects the draw path. The canvas resizes with the window via `sizeCanvas()`.

## Committing

Always commit changes after completing a change, with a meaningful message that explains what changed and why (not just "update files"). Push to `origin/main` so the deployed GitHub Pages site stays current.

## Deploying

Hosted as a public GitHub repo. Since it's a static single file, GitHub Pages can serve it directly with no configuration.
