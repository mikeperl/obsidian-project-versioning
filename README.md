# Project Versioning

Project Versioning creates complete ZIP snapshots of projects and shows exactly what changed between versions. It is intended for people who want understandable project-level version history without adopting Git.

## Features

- Version an entire vault, a vault subfolder, or another desktop folder.
- Create complete ZIP snapshots named by date and version number.
- Compare each new snapshot with the preceding snapshot.
- Detect unchanged, modified, added, deleted, moved, and renamed files.
- Generate line-level unified diffs for text files and hashes for binary files.
- Classify changes as substantive, metadata-only, or generated/derived.
- Produce Markdown and JSON comparison reports plus a persistent version history.
- Manage old snapshots from inside Obsidian.
- Detect and remove consecutive duplicate snapshots while preserving the newest copy of each unchanged run.
- Remove a project while keeping its version history, or explicitly delete the versioning data created for that project.
- Apply optional retention policies with a preview before manual cleanup.
- Use multiple independently versioned projects in one vault.
- Edit existing project definitions, including moved or renamed source folders, with dirty-state save handling and unsaved-change protection.
- Validate project, storage, and export paths before saving, prevent source/storage tree overlap, and reject duplicate active source-folder configurations.
- Complete folder paths while typing and browse the filesystem with explicit current-folder, parent, drive, and selection controls.
- Open configured folders in the operating system file manager directly from the project editor.
- Copy the latest or any chosen snapshot to a convenient export folder outside hidden storage.

Project Versioning is desktop-only and has no runtime dependencies. No Python, shell tools, Git installation, or external diff program is required.

## Basic workflow

```text
Edit project
→ Archive and compare
→ versioned ZIP snapshot
→ comparison with previous snapshot
→ Markdown + JSON audit report
→ version history
```

The first run creates the baseline snapshot. Later runs create a new snapshot and compare it with the previous one.

## Installation

### Community Plugins

After the plugin is accepted into the Obsidian Community directory:

1. Open **Settings → Community plugins**.
2. Search for **Project Versioning**.
3. Install and enable it.

### Manual installation

Download `main.js`, `manifest.json`, and `styles.css` from a matching GitHub release and place them in:

```text
<Vault>/.obsidian/plugins/project-versioning/
```

Reload Obsidian and enable **Project Versioning** under Community plugins.

## Documentation

See the [complete user guide](docs/User%20Guide.md) for project configuration, snapshots, comparisons, retention, exporting, cleanup, and illustrated interface guidance.

## First use

1. Open **Settings → Project Versioning**.
2. Select **Add project**.
3. Name the project.
4. Choose the vault root or a vault folder, or enter an absolute desktop folder.
5. Leave **Storage folder** blank to use the automatic private storage location.
6. Start typing a folder path for hierarchical autocomplete, or use **Vault...** / **Browse...** for project, managed-storage, and snapshot-export locations.
7. Optionally configure a **Snapshot export folder** if you want convenient copies of ZIP snapshots elsewhere on the computer.
8. Save the project.
9. Run **Archive and compare project** from the Command Palette or use the archive ribbon icon.

When several projects are configured, commands open a searchable project picker.

## Commands

- **Archive and compare project**
- **Manage snapshots**
- **Compare latest snapshots**
- **Compare chosen snapshots**
- **Open latest comparison report**
- **Open version history**
- **Add versioned project**
- **Edit versioned project**
- **Export latest snapshot**
- **Export chosen snapshot**

No default hotkeys are assigned. Users can assign their own under **Settings → Hotkeys**.

## Snapshot storage

By default, project versions are stored under the current vault:

```text
.project-versions/<project-id>/
├── archives/
└── reports/
```

This storage directory is automatically excluded from snapshots, preventing recursive archive growth. A project can instead use a vault-relative or absolute storage path.

### Changing managed storage

Folder-location fields provide compact path-aware autocomplete, a searchable vault-folder selector, a filesystem folder browser, direct text entry, live validity status, and an **Open** button for existing folders. Focusing a path field shows up to six valid matching folders at the current level; hierarchical typing narrows the list one path segment at a time. Candidates that fail validation are omitted rather than shown as unusable choices. Hidden/internal dot-prefixed folders and Windows system folders are not offered. The filesystem browser is dedicated to visual navigation, always shows the full current path, lists only accessible Windows drives, and disables folder selection when the current location cannot be opened. Missing storage/export folders are identified before save and may be created when needed; the project source itself must already exist.

A source folder already used by another active project is rejected; one folder cannot be registered as the source of two active versioned projects. Managed storage is also strict: custom managed storage must be completely separate from its own project source and every active project source, and managed-storage trees may not overlap one another. Custom paths inside hidden vault folders are rejected; Windows system folders such as `$RECYCLE.BIN` and `System Volume Information` are also unavailable. Leave Storage folder blank to use the automatic private storage location.

Changing an existing project's managed storage folder migrates and verifies the complete archive/report history before the setting is saved. Returning from custom storage to **Default** always returns that project to the same `.project-versions/<project-id>` identity. If a removed project kept its default managed history and the same source folder is later re-added, Project Versioning reconnects to that retained default lineage instead of creating a numbered replacement folder. Non-empty unrelated destinations are rejected before save rather than merged or overwritten.

> [!IMPORTANT]
> Snapshots stored on the same disk as the source project provide version history, not disaster recovery. Keep a separate backup if you need protection against disk loss, theft, or filesystem corruption.

If a sync service synchronizes the entire vault, it may also synchronize the default ZIP archives. For large or frequently versioned projects, an external storage folder can avoid that additional synchronization.

## Editing projects and exporting snapshots

Existing projects can be edited from **Settings → Project Versioning → Edit** or with the **Edit versioned project** command. Changing the source folder preserves the project ID and therefore preserves its managed archive location and version history. This is the intended way to update a project after its folder has been moved or renamed.

For an existing project, **Save project** remains disabled until an effective setting actually changes and all blocking validation errors are resolved. Reverting the edits disables Save again. Closing the editor with unsaved effective changes prompts before discarding them.

Each project can optionally define a **Snapshot export folder**. The managed snapshot remains in `.project-versions` (or the configured storage folder), while export creates an ordinary copy with the same filename in the chosen vault-relative or absolute desktop folder. Use **Export latest snapshot** or **Export chosen snapshot** at any time. The automatic-copy toggle is shown only while a valid export-folder path is configured.

Export never silently overwrites an existing file. If the same snapshot already exists in the export folder with identical contents, Project Versioning reports that it is already exported and leaves the file unchanged. If the same filename exists with different contents, export is refused and the existing file is preserved.

If the export folder is located inside the project source tree, Project Versioning automatically excludes that folder from subsequent snapshots. This prevents exported ZIP files from being archived inside later ZIP files.

## Snapshot management and retention

Run **Manage snapshots** to inspect stored ZIPs, creation times, sizes, and total storage. Older snapshots can be selected and permanently deleted after confirmation. The newest snapshot is always protected and therefore has no deletion checkbox.

**Remove duplicates** scans snapshot contents rather than ZIP-container metadata. It treats only consecutive snapshots with exactly the same file paths and file contents as duplicates, deletes the older redundant copies after confirmation, and keeps the newest copy in each duplicate run. A later return to an earlier project state is not removed merely because its contents match a non-consecutive historical snapshot.

Retention is disabled by default. Available policies are:

- keep everything;
- keep the latest N snapshots;
- keep snapshots from the last N days;
- tiered retention: keep recent snapshots in full, then one per day for a configured period, then one per week.

The manager previews the number of snapshots and amount of storage affected before cleanup. Automatic cleanup after a successful snapshot is optional and must be explicitly enabled per project.

## Removing projects and deleting versioning data

Removing a project and deleting its stored versioning data are deliberately separate choices.

- **Remove only** removes the project from active versioning and leaves managed snapshots and reports untouched. Project Versioning retains the storage record so the data can still be deleted later from the **Retained versioning data** section in Settings. Retained history does not count as an active project registration, so the same source folder can later be added again.
- **Remove + delete data** permanently deletes that project's managed `archives/` and `reports/` trees and then removes the project definition. The actual project source folder is never deleted or modified.
- **Delete all stored versioning data** under **Settings → Project Versioning → Storage and privacy** permanently deletes managed snapshots and reports for all active projects plus data retained from removed projects, and also scans the default `.project-versions` namespace for orphaned managed data left by older plugin versions. The action is disabled when there is no managed data to delete. Active project definitions and all project source files are preserved. This is the cleanup operation to use before uninstalling if you do not want Project Versioning data left behind. If the plugin remains installed, active projects stay configured but their next snapshot starts a new history.

For safety, cleanup removes only Project Versioning's managed `archives/` and `reports/` subfolders. A custom storage folder itself is removed only when it is empty afterward; unrelated files or folders are preserved.

Snapshot export copies are ordinary user-facing files and are preserved by default. Destructive cleanup dialogs provide an optional **Also delete known exported snapshot copies** toggle. When selected, the plugin removes only snapshot filenames known from that project's managed archive/history and only from the currently configured export folder. Copies in previous export locations, renamed copies, and unrelated files are not removed automatically.

Uninstalling or disabling Project Versioning does **not** automatically delete `.project-versions` or configured external managed-storage folders. Automatic deletion on uninstall is intentionally avoided because those snapshots may be the user's only remaining version history. Use the explicit cleanup controls when deletion is desired.

## Version history

**Open version history** shows a compact table with Snapshot, Created, Previous, Status, and Changes. Creation times use the sortable local format `YYYY-MM-DD_HH:mm_DDD` (for example `2026-08-28_00:44_FRI`). Change counts and the archive SHA-256 are available by expanding the Changes cell instead of occupying permanent columns.

## Comparison behavior

The comparator uses SHA-256 hashes to identify equal file contents. Text files below the configured size threshold receive line-level diffs. In the Obsidian report view, paired removed and added lines additionally emphasize the exact changed characters, including changes inside very long generated lines. Binary and oversized text files are still hashed and reported as changed.

Exact content moves/renames are detected independently of path. A similarity detector can also recognize text files that were moved or renamed and edited.

### Change significance

Ordinary modifications are substantive by default. A project can optionally define regular expressions for metadata-only lines. A changed text file is classified as metadata-only only when every changed nonblank line matches a configured pattern.

Example:

```text
^\s*last_updated\s*:
^\s*synchronized_with\s*:
```

Generated or derived paths can be identified with globs such as:

```text
Build/**
Exports/**
Generated/**
```

The classifier is deliberately conservative: uncertain modifications remain substantive.

## Default exclusions

Project Versioning excludes common non-project or self-generated content, including:

```text
.project-versions/**
.trash/**
.git/**
<vault-config-dir>/workspace.json
<vault-config-dir>/workspace-mobile.json
**/.DS_Store
**/Thumbs.db
**/__pycache__/**
**/*.tmp
```

The actual Obsidian configuration directory is obtained from the vault at runtime; it is not assumed to be `.obsidian`. Projects can add additional exclusion globs.

## Data access and privacy disclosures

Project Versioning is designed for local, offline use.

- **Network:** The plugin makes no network requests.
- **Telemetry:** The plugin collects no telemetry or analytics.
- **Accounts:** No account or external service is required.
- **Advertising/payments:** The plugin contains no ads, purchases, or payment features.
- **Vault access:** The plugin reads project files selected by the user and writes local ZIP archives and reports.
- **Files outside the vault:** External filesystem access is optional. It occurs only when the user explicitly selects or enters an absolute project, managed-storage, or snapshot-export path. External project paths are read as versioning sources; external managed-storage paths receive the plugin's `archives/` and `reports/` data; external export paths receive user-requested snapshot copies. Project Versioning does not scan unrelated filesystem locations.
- **Why external access exists:** Optional external managed storage lets users keep version archives physically separate from the vault and from vault synchronization or backup processes. External project paths also allow a vault to version a desktop project located elsewhere.
- **Deletion:** Snapshot deletion and retention cleanup remove only selected managed ZIP snapshots. Project-level and global cleanup can explicitly delete managed `archives/` and `reports/` data; project source files are never deleted. Unrelated content in custom storage folders is preserved. Known exported snapshot copies are deleted only when the user explicitly selects that option.
- **Uninstall cleanup:** Disabling or uninstalling the plugin does not automatically delete `.project-versions` or external managed storage. Use the explicit cleanup controls before uninstalling if those files should be removed.
- **Cloud:** Project contents are never uploaded by the plugin. A separate sync or backup service may independently copy files placed inside a synchronized vault.
- **External programs:** Normal versioning, comparison, export, and cleanup do not execute external applications. The explicit **Open** folder buttons launch only the configured existing folder in the operating system file manager. This uses direct process arguments with no command shell and performs no network access.

Symbolic links encountered while scanning a project are skipped rather than followed.

## Current limits

- A single file of 4 GiB or larger is rejected. Aggregate archives can exceed 4 GiB using ZIP64 central-directory support.
- Line diffs are generated only for text files below the configured diff-size limit; larger files are still detected by hash.
- Modified-move detection uses line-content similarity rather than semantic analysis.
- Snapshot creation is not transactional with simultaneous edits. Avoid editing important project files while a release snapshot is being created.

## Development

Requirements: Node.js 20 or later. The repository includes `package-lock.json`; use reproducible clean installs.

```bash
npm ci
npm audit
npm run check
```

For active development:

```bash
npm run dev
```

Production builds generate a minified `main.js`. The repository intentionally ignores `main.js`; release builds attach it to GitHub releases instead.

See [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md) and [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Security

See [`SECURITY.md`](SECURITY.md). Please do not post private vault contents, sensitive paths, or unredacted comparison reports in public issues.

## License

MIT.
