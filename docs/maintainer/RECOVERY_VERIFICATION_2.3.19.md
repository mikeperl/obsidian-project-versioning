# Project Versioning v2.3.19 — Recovery verification and release hold

**Date:** 2026-10-10. **Status:** source correction and Linux automated safety tests completed; NOT approved for public release.

## Addressed findings from the v2.3.18 CPD audit

| Audit issue | v2.3.19 implementation |
|---|---|
| C1 source deletion through storage aliases | Reject managed-storage/junction/symbolic-link aliases and realpath overlap; validate before snapshot, comparison, export, migration, deletion, and history mutation; verify isolation from all configured projects in the Obsidian integration |
| C2 export collision deletion | Only delete when the corresponding managed ZIP exists and has identical length and SHA-256; preserve/report unverified candidates |
| H1 snapshot version reuse | Reserve numbers from archive files AND historical Version History entries, including missing/deleted records |
| H2 malformed ZIP and interrupted archive | Structural ZIP checks, valid-only automated snapshot selection, CRC/size verification during entry hashing, explicit malformed listing; rollback unrecorded ZIP and sidecars on pre-history failure |
| H3 unrelated files moved by migration | Transfer/verify only managed `archives/` and `reports/`; preserve unrelated old storage-root entries |
| H4 export/managed overlap | Reject equal, parent, child, and physical alias paths |
| H5 missing history | Reconcile `Available`, `Malformed`, and `Missing` while preserving `Deleted` and historical identity |
| P1 source filesystem roots | Reject actual filesystem roots (including Windows drive roots when run on Windows) |
| P2 source change warning | Show nonblocking continuity warning when the physical source changes |
| P3 repeated linked logical paths | Include each user-approved linked path in ZIP; unique physical size counted once as required by CPD Iteration 2 |
| P4 excluded linked largest entries | Report detected largest linked file and folder even when excluded |
| P6 first snapshot report | Display Archive Statistics after first snapshot; later snapshots show comparison report with statistics or statistics alone |
| P7 CPD implementation mismatch | Clarified by append-only CPD iteration: no separate `archive-statistics-modal.ts` exists in reconstructed v2.3.18/2.3.19; shared ReportModal renders Archive Information |
| P8/P9 historic versioning and artifact history | Historic v2.3.18 patch numbering cannot be retroactively corrected; version 2.3.19 metadata synchronized and artifact precisely identified in CPD |
| P10 documentation | Updated README, User Guide, Changelog, development guidance, release hold checklist, and manual-testing addendum |

## Execution results in Linux environment

- Source ZIP was previously audited for integrity and the supplied v2.3.18 baseline identified by SHA-256 `a36cc030e77477b1463c4a95ce62dfe20a28e9f790a358f35513c14e23403952`.
- Node.js v22.16.0. TypeScript (`tsc --noEmit --skipLibCheck`): PASS.
- ESLint (`eslint src tests --max-warnings=0`): PASS.
- Repository submission audit: PASS.
- Core suite, including new filesystem safety tests: PASS using a transient TypeScript-to-CommonJS transpilation harness; normal bundled esbuild runner was not available on Linux.
- Generated `main.js` through `scripts/recovery-bundle.mjs`: PASS syntax, mocked Obsidian API load PASS. **This is not a minified production build.**
- `npm run check`: FAILS at esbuild (`@esbuild/win32-x64` present, `@esbuild/linux-x64` missing); standard complete pipeline not verified.
- `npm audit --offline`: reported zero **cached** vulnerabilities only; does not replace a live advisory audit.
- npm registry lookup for esbuild: failed DNS resolution (`EAI_AGAIN`); clean `npm ci` and live `npm audit` not verified.
- Windows junctions and Obsidian UI: NOT TESTED here. Windows+Ubuntu CI: NOT OBSERVED. Release workflow and published artifacts: NOT TESTED.

## Remaining questions and release blockers

1. **CPD semantic conflict P5:** `Exclude` prevents external files entering ZIP, but target contents are still enumerated to calculate required pre-snapshot sizes. The frozen CPD also says Exclude must not traverse linked-folder contents. This wording must be resolved by the project owner without silently editing prior iterations.
2. The structural validity check does not decompress every stored ZIP at each listing; payload corruption beyond the directory/header may be discovered during actual entry hashing/CRC validation.
3. Explicit Windows and Obsidian testing remains necessary for junction behavior, file manager interaction, multi-drive migration and destructive recovery.
4. Reinstall clean dependencies in a suitable build environment; run `npm ci`, live `npm audit`, `npm run check`, Windows and Ubuntu CI; verify the exact minified release artifacts.
5. Public release is not authorized. Do not tag/publish this candidate as 1.0.0 or a finished Community-plugin release.

## Data safety

Exercise destructive operations on disposable data only. A cleaned test vault does not demonstrate absolute freedom from time-of-check/time-of-use filesystem races, network filesystem changes, or all host-specific edge cases. Until Windows/Obsidian verification passes, preserve independent backups before evaluating this test build.
