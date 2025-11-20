<!-- .github/copilot-instructions.md: guidance for AI coding agents working on this repo -->
# Copilot instructions — Fluxo (quick, actionable)

Purpose
- This repo is a single-page static visualization and analysis tool for network max-flow (Ford–Fulkerson) implemented in `index.html`.
- Keep changes minimal and focused: the code is intentionally self-contained (HTML + CSS + JS in one file).

Quick start (developer workflow)
- Open `index.html` in a browser. No build step or package manager is required.
- Recommended dev convenience: use VS Code Live Server or any simple static file server to get auto-reload.

Key files & patterns
- `index.html` — everything: styles, canvas UI, state and algorithm implementation.
  - Important functions: `drawGraph`, `runFordFulkerson`, `stepFordFulkerson`, `calculateMinCut`, `loadExample`.
  - Key data shapes: `nodes` array ({id,label,x,y}), `edges` array ({id,from,to,capacity,flow}), and `algorithmState` ({flow, residual, augmentingPaths, stepsLog, finished}).
- `.vscode/settings.json` and `.gitattributes` are present but there is no CI/Build config.

What to know before editing
- This is a single-file app — large structural edits (split into modules, add bundler) should be proposed as a PR with an incremental migration plan.
- Preserve the public DOM ids and function names when possible because the UI and inline handlers rely on them (e.g., buttons call `runFordFulkerson()` via `onclick`).
- State is global and mutated directly; small changes should keep the same state shape to avoid breaking the UI.

When an AI makes edits
- If `.github/copilot-instructions.md` already exists: merge by preserving existing human-written guidance and append any new, repo-specific facts. Do not overwrite without preserving prior content.
- For non-trivial refactors, produce a short migration plan and tests/examples: (1) extract canvas drawing into `src/draw.js`, (2) move algorithm into `src/ff.js`, (3) add a minimal `package.json` and dev server — but do this in small commits so reviewers can follow.

Examples of useful, precise prompts for this repo
- "Extract the Ford–Fulkerson logic from `index.html` into a module `ff.js` keeping the same API (runFordFulkerson, stepFordFulkerson) and update `index.html` to import it." 
- "Create a small test harness that loads `loadExample(2)` and asserts `runFordFulkerson()` yields expected max flow for that example. Keep changes isolated and explain required test runner." 

Do not assume
- There is no backend, no package.json, no tests, and no CI. Don't add assumptions about frameworks or existing build pipelines.

Where to look for behavior to preserve
- Any inline `onclick` attributes (e.g., `onclick=\"runFordFulkerson()\"`) — changing them requires updating the DOM wiring.
- `nodes`/`edges` shapes and how `drawGraph()` reads them to draw the canvas and labels.

If you need more info
- Ask for permission before introducing a build system or splitting the single file; request a short migration plan and target branch.

Please review: any unclear or missing repo-specific detail I should add? (e.g., preferred branch naming, target browsers, or deployment host information.)
