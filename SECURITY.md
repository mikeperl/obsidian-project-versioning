# Security policy

Project Versioning is a desktop-only plugin that reads and writes local files through Node.js filesystem APIs.

## Intended access

The plugin:

- reads configured project folders and, if the user authorizes including linked directory contents, the linked targets;
- writes ZIP archives and comparison reports only to the configured project storage location;
- can access files outside the vault only when the user explicitly configures an absolute project, storage, or snapshot-export path;
- skips file symbolic links and only follows directory links/junctions when the configured linked-folder policy permits it, with cycle protection;
- makes no network requests;
- has no telemetry, analytics, accounts, advertising, or cloud upload;
- does not execute a command shell; the explicit **Open** folder buttons may launch the operating system file manager for an already configured local folder.

Two active or retained project definitions are prevented from sharing or overlapping managed-storage trees because shared storage would mix snapshot lineages and complicate safe cleanup. Custom managed storage is also kept separate from active project source trees. Hidden vault folders and known Windows system folders such as `$RECYCLE.BIN` and `System Volume Information` are not offered by folder selectors and are rejected when entered directly.

Snapshot cleanup can permanently delete ZIP archives in the configured storage location. Manual deletions require confirmation, automatic cleanup is disabled by default, and the newest **valid** snapshot is protected. Export-copy deletion is optional and subject to additional ownership checks.

## Reporting a vulnerability

If GitHub private vulnerability reporting is enabled for the repository, use **Security → Report a vulnerability**. Otherwise, open a public issue requesting a private contact method without including exploit details, private file paths, or vault contents.

## Folder opening

The **Open** buttons in the project editor are user-initiated convenience actions. Before launch, the plugin verifies that the target exists and is a directory. On Windows, it starts `explorer.exe` directly with an argument array and requests a new Explorer window; on macOS and Linux, it delegates to Electron's native `shell.openPath()` API. Launch failures are surfaced to the user. No command shell is used.

## Snapshot export

A project can optionally copy snapshots to a user-configured local folder. Export is local-only and performs no network transfer. If the export folder is inside the project source tree, it is excluded from subsequent snapshots.

## Backup completeness

Selective snapshots intentionally omit files according to the configured policy and should not be treated as complete vault backups. In particular, the optional Obsidian configuration-folder exclusion omits plugin settings and other vault configuration files. A separate complete backup is recommended for recovery from device loss or corruption.
