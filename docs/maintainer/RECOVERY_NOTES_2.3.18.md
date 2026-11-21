# Project Versioning v2.3.18 — Recovery notes

The user-supplied source ZIP and alleged compiled-plugin ZIP contained identical `main.js`, `manifest.json`, and `styles.css`. Neither contained the later linked-folder features visible in the screenshots. The original lost TypeScript implementation therefore could not be extracted from these files.

This package reconstructs the linked-folder prompt, per-project Ask/Exclude/Include policy, linked target scanning, deduplication, exclusion-aware ZIP creation, and archive information reports from the screenshots and prior documented requirements. It is **not a byte-identical recovery**.

## Verification performed

- TypeScript type checking: passed (`tsc --noEmit --skipLibCheck`).
- ESLint: passed.
- Core regression suite: passed using TypeScript compilation to CommonJS (the supplied esbuild binary is Windows-only, while recovery took place on Linux).
- New tests: real symbolic links, duplicate targets, nested link cycles, include/exclude policies, actual ZIP entry names, and comparison reports.
- Recovery fallback JavaScript bundle: Node syntax check and module loading with mocked Obsidian API.

## Still required

1. On Windows, install standard dependencies using `npm ci` and run `npm run check` (normal esbuild toolchain).
2. Install the generated `main.js`, `manifest.json`, and `styles.css` in a test vault; manually test the junction prompt and archive reports.
3. Do not publish until Windows-specific behavior, Obsidian UI, archive extraction, and size calculations have been verified.
4. Never include live `data.json` from installed plugins in public release packages: it contains private project configuration and paths.

`scripts/recovery-bundle.mjs` is a temporary, pure-TypeScript-compiler fallback. Normal development and release builds should continue to use the existing esbuild configuration.
