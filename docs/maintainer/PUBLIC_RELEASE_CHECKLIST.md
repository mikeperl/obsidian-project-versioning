# Public release checklist — 2.3.17

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
- [x] Production output is minified.
- [x] No telemetry or network requests.
- [x] External file access, external-program launch, and deletion behavior are disclosed.
- [x] Plugin settings use `Plugin.loadData()` / `Plugin.saveData()`.

## Before creating the release tag

- [ ] Extract or clone into a clean working directory on Windows.
- [ ] Run `npm ci`.
- [ ] Run `npm audit` and confirm zero known vulnerabilities.
- [ ] Run `npm run check`.
- [ ] Confirm `manifest.json` and `package.json` both contain `2.3.17`.
- [ ] Complete the applicable final manual tests in a disposable Obsidian desktop vault.
- [ ] Push `main` and confirm CI passes on Ubuntu and Windows.
- [ ] Recheck that `project-versioning` and `Project Versioning` remain unique in the Community directory.

## Release and release-asset verification

- [ ] Create annotated tag `2.3.17` with no `v` prefix and push it.
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
