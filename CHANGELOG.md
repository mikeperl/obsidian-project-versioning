# Changelog

## 2.3.17 — Retention progressive disclosure

- Development security: updated the locked `fast-uri` transitive dependency from 3.1.5 to 3.1.6 and renamed the repository-specific audit script to `submission-audit` so it cannot be confused with npm's dependency security audit.
- Simplified the Add/Edit Project retention interface using progressive disclosure. When retention is disabled, its inactive configuration fields are hidden instead of remaining visible.
- Enabling retention now reveals the retention mode and automatic-cleanup control, while only the fields relevant to the selected mode are shown. Changing modes updates those fields immediately.
- Added regression coverage for disabled, keep-all, latest-count, age-days, and tiered retention-control visibility states.
- Updated retention documentation and manual-test coverage for the conditional UI; the obsolete retention screenshot is no longer embedded until it is recaptured from the host application.
- Replaced conditional setting visibility implemented through inline JavaScript style assignment with a plugin CSS class, preserving the same retention/export behavior while following Obsidian release guidance.
- Prepared the public release workflow to generate build-provenance attestations and create a draft GitHub release for human inspection before publication.

## 2.3.16 — Duplicate project-folder validation

- The filesystem folder picker now keeps the complete **Current folder** path available on one line with horizontal scrolling instead of truncating it with an ellipsis, while preserving the fixed-height modal layout.
- Fixed **Delete stored data...** so it is disabled when no managed archives/reports exist, becomes enabled when stored versioning data exists, and returns to disabled after cleanup removes the last managed data.
- A folder already registered as the source of an active versioned project is now a blocking validation error rather than a warning. The project cannot be saved with the same source folder under a second active project.
- The desktop folder picker now disables **Use current folder / Use selected folder** for an already-versioned project folder and for any other invalid candidate.
- The picker action starts disabled and is disabled again while each asynchronous folder validation is pending, preventing a fast click from accepting a newly selected folder using stale validation state.
- Historical projects whose versioning was removed but whose archives were retained remain eligible to be re-added; retained storage continues to participate in storage-conflict protection.
- Serializes Add/Edit project persistence, recomputes new-project identity, and revalidates against current settings immediately before saving. This prevents stale or simultaneously saved project dialogs from bypassing duplicate-source protection, storage conflicts, or ID uniqueness.
- Added regression coverage for duplicate-source blocking, stale-editor revalidation, and folder-picker validation-state safeguards.

## 2.3.15 — Escape handling correction

- Corrected the Add/Edit Project dialog Escape behavior when a path autocomplete list is open. The modal now consumes its own close request while the focused path field still has an open suggestion list, preventing Obsidian's modal-level Escape handler from closing the parent dialog before the autocomplete handler runs.
- First **Escape** closes only the suggestion list; a subsequent **Escape** performs the normal dialog close behavior.
- Strengthened the UI regression check to require the modal-close guard rather than testing only the autocomplete key handler.

## 2.3.14 — Duplicate snapshot cleanup and Escape handling

- Added **Remove duplicates** to the Snapshot Manager. It identifies consecutive snapshots with exactly identical archived file paths and contents, removes the older redundant copies only after confirmation, and preserves the newest copy in each unchanged run.
- Duplicate detection intentionally does not collapse non-consecutive matching states, preserving meaningful history when a project later returns to an earlier state.
- Duplicate cleanup uses the existing guarded snapshot-deletion path, including report cleanup, version-history status updates, latest-report refresh when needed, and newest-snapshot protection.
- Fixed folder-path autocomplete Escape handling in the Add/Edit Project dialog: the first **Escape** closes an open suggestion list only; a subsequent **Escape** can close the project dialog.
- Added regression coverage for consecutive duplicate detection, non-consecutive repeated-state preservation, newest-copy preservation, and project-editor Escape interception.

## 2.3.13 — Filename-safe project identity validation

- Project names and explicit archive filename prefixes are now validated before saving against the cross-platform filename policy used by the plugin. Invalid Windows filename characters (`< > : " / \ | ? *`), control characters, leading/trailing whitespace, trailing periods, `.`/`..`, and reserved Windows device names are rejected instead of silently rewritten.
- Validation is shown directly on the **Project name** and **Archive filename prefix** settings, and **Save project** remains disabled until both values are valid. Snapshot creation also refuses any legacy persisted configuration that violates the policy instead of silently sanitizing it.
- Removed the heuristic project-name repair introduced in 2.3.12. Stored project names are preserved exactly; the plugin now validates user input rather than guessing whether a legitimate name looks malformed.
- Leaving **Archive filename prefix** blank now remains a real blank configuration value, so future snapshots continue to use the current project name as documented instead of silently freezing the project name into the prefix at save time.
- Dirty-state tracking now preserves exact project-name and archive-prefix edits, so invalid whitespace or prefix edits still trigger unsaved-change protection even though they cannot be saved.
- Added regression coverage for filename-safe names, Unicode and punctuation that are valid, all forbidden filename characters, control characters, reserved Windows names, trailing whitespace/periods, blank-prefix semantics, and direct project-draft validation.

## 2.3.12 — Filesystem reliability and storage-lineage fixes

- Windows **Choose drive** now enumerates only drive roots that currently exist and can be read instead of displaying every possible drive letter.
- The filesystem browser now enters one unambiguous error state when a folder cannot be opened: stale child rows are cleared, **Use current folder** is disabled, and a successful `Folder found` message is never shown at the same time.
- Replaced the previous detached file-manager process launch with Electron's native `shell.openPath()` for the Project, Storage, and Snapshot export **Open** actions, with returned launch errors surfaced to the user.
- Fixed retained-history reactivation: re-adding a source folder whose default-storage history was retained reuses the retained project identity and `.project-versions/<id>` lineage instead of silently allocating `<id>-2`.
- Added regression coverage proving that an existing project moved `Default → custom → Default` returns to exactly the same default managed-storage path.
- Autocomplete now omits candidates that fail the field's validation instead of displaying non-selectable `Unavailable` rows. It is limited to six valid suggestions and is sized to the path textbox.
- **Copy each new snapshot to export folder** is hidden unless a valid export-folder path is configured. Clearing the export path also clears the automatic-export flag, including stale persisted configurations when settings are loaded.
- Project names must contain at least one Unicode letter or number. Malformed persisted names such as punctuation-only values are repaired in memory from the source-folder name, preventing corrupted labels in project pickers.
- Added regression coverage for drive enumeration, retained-lineage reactivation, malformed project names, valid-only autocomplete, conditional export controls, native folder opening, and browser error-state handling.

## 2.3.11 — Path-selection correction and system-folder safeguards

- Reworked folder autocomplete into a compact dropdown anchored to the path textbox instead of a modal-width panel. The menu has no horizontal scrolling, uses concise folder/context rows, shows a small `No matching folders` state, and opens on focus so visible folders can be chosen without first remembering a name.
- Hidden/internal dot-prefixed folders such as `.obsidian`, `.project-versions`, and `.git` are omitted from vault selection, filesystem browsing, and autocomplete suggestions.
- Windows system folders `$RECYCLE.BIN` and `System Volume Information` are omitted from browsing/autocomplete and rejected when typed directly as project, managed-storage, or snapshot-export locations.
- Removed the redundant **Open a path** row and **Open path** button from the filesystem browser; direct path entry remains in the main project editor while Browse is now dedicated to visual navigation.
- Preserved the existing **Current folder**, **Parent folder**, **Choose drive**, **Cancel**, and context-sensitive **Use current/selected folder** workflow.
- Added regression coverage for hidden/system-folder filtering, direct-entry rejection, hierarchical autocomplete, and autocomplete CSS overflow constraints.

## 2.3.10 — Storage isolation and folder navigation

- Enforced disjoint custom managed-storage trees: custom storage cannot be the same as, inside, or contain its own project source, and cannot overlap any active project's source tree.
- Managed-storage trees may no longer overlap each other, including parent/child relationships across active or retained projects.
- Custom storage inside Project Versioning's internal `.project-versions` tree is rejected; leaving Storage folder blank remains the supported way to use default managed storage.
- Preserved the deliberate whole-vault exception for automatic default storage under `.project-versions`, which is excluded from snapshots.
- Redesigned the filesystem folder picker with a fully visible current path, **Open path**, **Parent folder**, **Choose drive**, **Cancel**, and context-sensitive **Use current/selected folder** actions.
- Folder rows now use conventional single-click selection and double-click/Enter navigation, and invalid storage choices are identified inside the filesystem picker before they can be selected.
- Added path-aware folder autocomplete to Project folder, Storage folder, and Snapshot export folder fields. Suggestions follow the typed hierarchy one path segment at a time, support keyboard selection, and show validation state where relevant.
- New projects now derive the Project name from an explicitly selected source folder until the user edits the name manually.
- Added a **Default** action and clearer wording for returning managed storage to `.project-versions/<project>`.
- Added regression coverage for source/storage tree overlap, managed-storage parent/child conflicts, internal-storage selection, whole-vault default storage, nested folder completion, and automatic project-name derivation.

## 2.3.9 — Version-history and removed-project fixes

- Replaced the crowded rendered Markdown Version History view with a dedicated compact table showing Snapshot, Created, Previous, Status, and Changes.
- Added expandable change details for file counts, substantive/metadata/generated modifications, additions, deletions, and SHA-256 without permanent extra columns.
- Added sortable local creation timestamps in `YYYY-MM-DD_HH:mm_DDD` format, for example `2026-08-28_00:44_FRI`.
- Existing Version History files are migrated automatically; exact creation times are recovered from retained ZIP timestamps when available, while deleted legacy entries fall back to the known date and weekday without inventing a time.
- The Version History header remains visible while scrolling, long snapshot references are truncated with full-name tooltips, and normal desktop use no longer requires horizontal scrolling.
- Removed the deletion checkbox entirely from the newest protected snapshot row instead of showing a disabled interactive-looking control.
- Fixed source-folder duplicate detection so data retained from a removed project no longer makes that folder appear actively versioned. Re-adding the same source folder is allowed while retained managed-storage conflicts remain protected.
- Added regression coverage for retained-project re-add validation, retained-storage conflicts, creation-timestamp formatting, compact history parsing, and migration of legacy history rows.

## 2.3.8 — Project-editor polish and defensive validation

- Added live validation for project, managed-storage, and snapshot-export paths, including clear existing/missing-folder status before save.
- Added blocking validation for unsafe path conflicts, non-empty storage-migration destinations, invalid metadata-only regular expressions, and managed-storage folders already used by another active or retained project.
- Added a non-blocking warning when a source folder is already versioned by another project.
- Added **Open** actions beside project, storage, and export paths; they open only existing folders and invoke the operating system file manager without a shell.
- Added unsaved-change protection when closing an Add/Edit Project dialog after making effective changes.
- Snapshot completion notices now say **no changes detected** for identical snapshots and include moved/renamed counts when changes exist.
- Project lists and project pickers are now sorted deterministically by project name, including immediately after settings are loaded.
- Confirmed and retained the Obsidian 1.13 declarative settings implementation; no legacy `display()` refresh path remains.
- Expanded regression coverage for missing folders, conflicting source/storage/export paths, duplicate storage, non-empty migration destinations, equivalent paths, Windows path casing, empty projects, Unicode filenames, project ordering, and unchanged-snapshot feedback.
- Updated documentation and security disclosures for live validation, discard protection, folder-opening actions, and duplicate-storage safeguards.

## 2.3.7 — Edit-project save-state correction

- The **Save project** button is now disabled when an existing project editor opens with no effective changes.
- Save becomes enabled only when a project setting actually differs from its saved value, and becomes disabled again if the user reverts the edit.
- Equivalent path spellings are normalized for change detection, so harmless differences such as trailing separators do not count as edits.
- Folder selections made through **Vault...** or **Browse...** participate in the same change detection.
- A defensive no-op guard prevents an unchanged edit from invoking storage migration, saving settings, or showing a misleading success notice.
- Added regression coverage for unchanged, equivalent-path, name, prefix, and retention-setting comparisons.

## 2.3.6 — Explicit data cleanup and uninstall safety

- Project removal now offers separate **Remove only** and **Remove + delete data** actions; neither action deletes or modifies project source files.
- **Remove only** retains a cleanup record so the preserved archives/reports remain discoverable under **Retained versioning data** instead of becoming orphaned hidden storage.
- Added per-retained-project cleanup and a global **Delete all stored versioning data** action for cleanup before uninstall.
- Destructive cleanup removes only managed `archives/` and `reports/` trees; unrelated files in custom storage folders are preserved, and storage folders are removed only when empty.
- Cleanup can optionally delete known exported snapshot copies from the currently configured export folder; exported copies remain untouched by default.
- Added protection against deleting a managed-storage tree that is simultaneously used by another active project.
- Expanded README and User Guide documentation for hidden-storage persistence, explicit cleanup, uninstall behavior, and the justification/boundaries for optional external filesystem access.
- Added core regression coverage proving that cleanup preserves project source files and unrelated custom-storage content.
- Corrected the submission audit so a locally generated ignored `main.js` is allowed; it now fails only when `main.js` is actually tracked by Git.

## 2.3.5 — Folder selectors

- Added folder-selection controls beside all project path fields while retaining direct text entry.
- **Vault...** opens the searchable in-vault folder picker for project, managed-storage, and snapshot-export paths.
- **Browse...** opens a cross-platform filesystem folder browser that supports empty folders, parent navigation, direct absolute-path navigation, and Windows drive selection.
- Browse selections inside the current vault are stored as vault-relative paths; selections outside the vault remain absolute.
- Added regression coverage for converting selected folders to the correct stored path form.

## 2.3.4 — Regression-test correction

- Corrected the archive-prefix rename regression assertion to compare the basenames of the snapshot path strings returned by `listSnapshots()`.
- No runtime behavior changed from 2.3.3; this release corrects the test harness so the intended prefix-continuity behavior can be validated by `npm run check`.

## 2.3.3 — Archive-prefix rename continuity

- Changing a project's archive filename prefix no longer disconnects existing snapshots from the project.
- Snapshot discovery, comparison, retention, export validation, and version numbering now treat the project-specific managed archive directory as the snapshot lineage; the prefix is filename presentation only.
- New snapshots use the current prefix while older snapshots retain their original filenames.
- Same-day version numbering continues across a prefix change (for example, `Project B ... V2.zip` followed by `Project B Renamed ... V3.zip`).
- Added regression coverage for project-name changes and archive-prefix changes.

All notable changes to Project Versioning are documented here.

## 2.3.2 — Windows external-drive migration fix

- Fixed `EPERM: operation not permitted, mkdir 'D:\\'` when migrating managed storage to a folder directly beneath a Windows drive root.
- Directory creation now checks whether a parent directory already exists before calling `mkdir`, avoiding Node.js' Windows drive-root `mkdir` failure.
- Managed storage can no longer be configured as a filesystem/drive root, preventing unsafe whole-drive cleanup semantics.
- Snapshot export validation uses the same root-safe directory creation helper.
- Added filesystem-root regression coverage and expanded CI to run the full check suite on both Ubuntu and Windows.

## 2.3.1 — Managed storage migration

- Changing a project's managed storage folder now migrates the complete snapshot/archive and report history instead of starting a disconnected archive lineage.
- Storage migration copies to a staging location, verifies file sizes and SHA-256 hashes, installs the verified copy, and only then removes the old managed-storage tree.
- Non-empty destination folders are treated as conflicts and are never silently merged or overwritten.
- Migration works across filesystem volumes, including Windows drive changes.
- Added regression coverage for V1–V4 migration followed by a V5 snapshot and for destination-conflict protection.

## 2.3.0 — Project editing and snapshot export

- Added a command-palette **Edit versioned project** action in addition to the existing Settings edit button.
- Project source paths can be updated while retaining the same project identity, archive storage, and version history.
- Added an optional per-project **Snapshot export folder** for convenient copies of managed ZIP snapshots.
- Added **Export latest snapshot** and **Export chosen snapshot** commands.
- Added optional automatic copying of every newly created snapshot to the configured export folder.
- Export folders located inside the source project are automatically excluded from future snapshots to prevent recursive growth.
- Existing v2.2 project settings migrate automatically with snapshot export disabled.
- Release-candidate hardening migrated the settings tab to Obsidian 1.13's declarative settings API, added typed settings deserialization, replaced deprecated destructive-button calls, and resolved the lint findings from the first local `npm run check`.

## 2.2.0 — Initial public release candidate

- Standalone desktop plugin; no Python, Git, shell tool, or external diff application required.
- Complete ZIP snapshots with `YYYY-MM-DD Vn` version naming.
- Text diffs, binary hashing, added/deleted/modified detection, and rename/move detection.
- Substantive, metadata-only, and generated/derived change classification.
- Markdown and JSON comparison reports plus persistent version history.
- Multiple independently versioned projects per vault.
- Whole-vault, vault-subfolder, and explicitly configured external-folder projects.
- In-Obsidian snapshot manager with manual deletion and optional retention policies.
- Hidden default storage under `.project-versions/<project-id>`.
- Public-release hardening: runtime configuration-directory handling, normalized user paths, sentence-case commands/UI, minified production builds, disclosure documentation, CI, release automation, and Obsidian-specific ESLint rules.

Versions 2.0.x and 2.1.x were private development builds and were not published to the Obsidian Community directory.

### 2.3.0 RC3 build compatibility
- Give Markdown report rendering its own Obsidian `Component` lifecycle so it satisfies the current `MarkdownRenderer.render()` API.
- Stop storing the snapshot-picker placeholder label as an unused class property.

### 2.3.0 RC4 build target
- Build the desktop-only plugin with esbuild `platform: "node"` so Node built-ins such as `node:fs`, `node:path`, `node:crypto`, `node:stream`, and `node:zlib` remain runtime-provided modules instead of being treated as browser dependencies.
