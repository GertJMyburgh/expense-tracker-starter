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

The original intentional string-amount bug has already been fixed: seed amounts in `src/App.jsx` are numbers and the form coerces input via `Number(amount)`, so the income/expense `reduce` calls now sum numerically.

## Architecture

Single-page React 19 app built with Vite. There is no backend, no persistence (state resets on page reload), and no routing.

Component layout (after the course's first round of decomposition):

- `src/App.jsx` — owns the `transactions` state and the `categories` list, defines the `handleAdd` callback (assigns `id` and `date`, then appends), and composes the three child components.
- `src/components/Summary.jsx` — receives `transactions`, derives `totalIncome` / `totalExpenses` / `balance` internally, renders the three summary cards.
- `src/components/TransactionForm.jsx` — owns its own form state (`description`, `amount`, `type`, `category`), validates non-empty description+amount, and calls `onAdd({ description, amount: Number(amount), type, category })`. Receives `categories` for the category select.
- `src/components/TransactionList.jsx` — owns its own filter state (`filterType`, `filterCategory`); filters are local because they only affect the list. Receives `transactions` and `categories`.

Data flow is one-way: `App` holds the source-of-truth list, child components either receive data via props or call back via `onAdd`. Form state and filter state are deliberately co-located with the components that use them rather than lifted into `App`.

Entry chain: `index.html` → `src/main.jsx` (mounts `<App />` in StrictMode) → `src/App.jsx`. Styles are split between `src/index.css` (global) and `src/App.css` (app-scoped).

ESLint uses the new flat config (`eslint.config.js`) with `js.recommended`, `react-hooks`, and `react-refresh/vite`. The custom rule `no-unused-vars` ignores identifiers matching `^[A-Z_]` (so unused PascalCase imports / SCREAMING_CONSTANTS don't error).

## Environment notes

Vite 7 requires Node ≥20.19 or ≥22.12. If `npm run dev` fails with `crypto.hash is not a function`, the active Node is too old — upgrade before debugging further.
