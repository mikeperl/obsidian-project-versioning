# Community submission audit — 2026-09-10

Candidate release: **2.3.17**

## Current Community directory uniqueness check

The official Obsidian Community Plugins list was checked on 2026-09-10. No exact match was found for:

```text
id: project-versioning
name: Project Versioning
```

This must be rechecked immediately before submission because the Community directory can change.

## Release requirements addressed

- Repository root contains `README.md`, `LICENSE`, and `manifest.json`.
- Manifest and package metadata use semantic version `2.3.17`; `versions.json` maps it to Obsidian 1.13.0.
- Plugin ID contains only lowercase letters and hyphens, does not contain `obsidian`, and does not end in `plugin`.
- Display name is short, uses Basic Latin, and contains neither “Obsidian” nor “Plugin”.
- Description is under 250 characters and ends with a period.
- `isDesktopOnly` is true because local Node/Electron APIs are integral to the plugin.
- Source repository ignores built `main.js`; releases attach `main.js`, `manifest.json`, and `styles.css` to the exact matching version tag.
- `package-lock.json` is present and must be committed with the public repository.
- Production builds are minified.
- No default hotkeys are assigned.
- Command names omit the plugin name and ID.
- The vault configuration directory is obtained at runtime rather than assuming `.obsidian`.
- `FileSystemAdapter` use is guarded by `instanceof`.
- Conditional UI styling uses plugin CSS classes rather than assigning inline styles from TypeScript.
- README discloses network behavior, telemetry, accounts, advertising/payments, local filesystem access, external-folder access, deletion behavior, external program launching, and cloud behavior.
- No client-side telemetry or network request library is present.
- The release workflow generates provenance attestations and creates a draft release for human inspection before publication.

## Remaining human verification

Automated/source checks cannot replace real Obsidian testing. Before publication, run a clean Windows install and the applicable final manual tests. After the GitHub release is published, install the exact downloaded release assets into a disposable vault for one final smoke test. Beta coverage on macOS and Linux remains desirable.
