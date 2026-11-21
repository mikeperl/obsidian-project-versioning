# Development verification and public release checklist — 2.3.19 (HOLD)

**Release hold:** v2.3.19 is a reconstructed development candidate. No public-release version/tag has been authorized; the fallback `main.js` is unminified and MUST be replaced by the exact tested minified production build before any release.

## Repository

- [ ] Public GitHub repository created.
- [x] `README.md`, `LICENSE`, and `manifest.json` are at repository root.
- [x] Source code is available at repository root.
- [x] `package-lock.json` is present for reproducible npm installs.
- [x] `main.js` is ignored and intended for releases only.
- [x] CI runs dependency audit, tests, submission audit, linting, and production build on Ubuntu and Windows.
- [x] Release workflow attaches `main.js`, `manifest.json`, and `styles.css`.
- [x] Release workflow generates build-provenance attestations and creates a draft release.

## Obsidian submission audit

- [x] Plugin name does not contain “Obsidian” or “Plugin”.
- [x] Plugin ID contains only lowercase letters and hyphens, does not contain `obsidian`, and does not end in `plugin`.
- [x] Command names do not repeat the plugin name or ID.
- [x] No default hotkeys.
- [x] Desktop-only status is declared because Node/Electron APIs are used.
- [x] `FileSystemAdapter` access is gated by `instanceof`.
- [x] No hardcoded `.obsidian` configuration-directory access.
- [x] No TypeScript/HTML inline styling; conditional UI styling uses plugin CSS classes.
- [x] Settings headings use the Obsidian `Setting` API.
- [ ] Exact v2.3.19 production `main.js` regenerated and verified minified in a clean build environment.
- [x] No telemetry or network requests.
- [x] External file access, external-program launch, and deletion behavior are disclosed.
- [x] Plugin settings use `Plugin.loadData()` / `Plugin.saveData()`.

## Before creating the release tag

- [ ] Reproduce the formerly destructive alias-cleanup and export-collision tests on a disposable Windows vault and confirm source/external files survive.
- [ ] Validate actual Windows junction handling and repeated-link inclusion, including disk sizes and path behavior.
- [ ] Test missing/malformed snapshots, version reservations, and failed-comparison cleanup through Obsidian.
- [ ] Verify storage migration preserves unrelated files in their original location across Windows volumes.
- [ ] Review the CPD's still-open Exclude-versus-size-enumeration wording with the project owner.

- [ ] Extract or clone into a clean working directory on Windows.
- [ ] Run `npm ci`.
- [ ] Run `npm audit` and confirm zero known vulnerabilities.
- [ ] Run `npm run check`.
- [ ] Confirm `manifest.json`, `package.json`, lockfile, and `versions.json` all identify the exact approved candidate (currently development v2.3.19).
- [ ] Complete the applicable final manual tests in a disposable Obsidian desktop vault.
- [ ] Push `main` and confirm CI passes on Ubuntu and Windows.
- [ ] Recheck that `project-versioning` and `Project Versioning` remain unique in the Community directory.

## Release and release-asset verification

- [ ] After owner approval, assign the approved public-release version, rebuild/retest the exact assets, and create the matching annotated tag (no `v` prefix).
- [ ] Confirm the release workflow passes.
- [ ] Confirm the draft release contains individual `main.js`, `manifest.json`, and `styles.css` assets.
- [ ] Confirm GitHub displays provenance attestations for the release assets.
- [ ] Review/edit release notes and publish the draft.
- [ ] Download the published assets into a fresh disposable plugin folder.
- [ ] Smoke-test the exact downloaded release assets in Obsidian.

## Community directory

- [ ] Sign in to the Obsidian Community directory and connect the GitHub account.
- [ ] Submit the public GitHub repository as a new plugin.
- [ ] Review the directory scanner results.
- [ ] Address any required corrections with a new incremented version and matching GitHub release.

## Optional beta coverage

- [ ] Test macOS.
- [ ] Test Linux.
- [ ] Exercise external folders, custom storage, migration, retention cleanup, and export on non-Windows platforms.
