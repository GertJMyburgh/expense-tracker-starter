# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm install` — install dependencies
- `npm run dev` — start Vite dev server at http://localhost:5173
- `npm run build` — production build to `dist/`
- `npm run preview` — serve the built `dist/` locally
- `npm run lint` — ESLint over all `**/*.{js,jsx}`

There is no test runner configured.

## Project context

This is the **starter** project for Mosh's Claude Code course (see README). It is intentionally shipped with bugs, poor UI, and messy code that the course refactors over time. Do not assume existing patterns are good — they are the starting point for refactoring exercises.

A known intentional bug: in `src/App.jsx`, transaction `amount` values are stored as strings (e.g. `"5000"`) but the income/expense totals use `reduce((sum, t) => sum + t.amount, 0)`, which produces string concatenation rather than numeric addition. Be aware before "fixing" totals — confirm with the user whether the lesson intends to fix it now.

## Architecture

Single-page React 19 app built with Vite. The entire application lives in **one component**: `src/App.jsx`. Everything — transaction state, form inputs, filter state, derived totals, and the rendered UI — is in that file using `useState`. There is no backend, no persistence (state resets on page reload), no routing, and no component decomposition yet.

Entry chain: `index.html` → `src/main.jsx` (mounts `<App />` in StrictMode) → `src/App.jsx`. Styles are split between `src/index.css` (global) and `src/App.css` (app-scoped).

ESLint uses the new flat config (`eslint.config.js`) with `js.recommended`, `react-hooks`, and `react-refresh/vite`. The custom rule `no-unused-vars` ignores identifiers matching `^[A-Z_]` (so unused PascalCase imports / SCREAMING_CONSTANTS don't error).

## Environment notes

Vite 7 requires Node ≥20.19 or ≥22.12. If `npm run dev` fails with `crypto.hash is not a function`, the active Node is too old — upgrade before debugging further.
