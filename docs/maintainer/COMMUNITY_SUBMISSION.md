# Community directory submission worksheet

Use this worksheet immediately before submitting the initial public release.

## Listing identity

```text
Name: Project Versioning
ID: project-versioning
Author: Michael Perlitz
Description: Create complete project snapshots, compare versions, and audit changes without Git or external tools.
Repository: <YOUR-GITHUB-USERNAME>/project-versioning
Release tag: 2.3.17
Minimum Obsidian version: 1.13.0
Desktop only: Yes
License: MIT
```

## Immediately before submission

- Recheck that `project-versioning` and `Project Versioning` are still available in the official Community directory.
- Confirm GitHub Issues and private vulnerability reporting are configured as intended.
- Confirm the public repository contains source, `README.md`, `LICENSE`, `manifest.json`, `versions.json`, and `package-lock.json`.
- Confirm `main.js` is not committed to the source repository.
- Confirm the GitHub release tag is exactly `2.3.17`, with no `v` prefix.
- Confirm the release contains `main.js`, `manifest.json`, and `styles.css` as individual assets and has provenance attestations.
- Install the exact assets downloaded from the published release into a clean desktop Obsidian vault and perform the final smoke test.
- Read the current Obsidian developer policies and plugin submission requirements once more in case they changed after this package was prepared.

## Suggested final smoke test

1. Add a small vault subfolder as a project.
2. Create its first snapshot.
3. Modify, add, delete, and rename test files.
4. Archive and compare again.
5. Inspect the comparison report and Version History.
6. Open the snapshot manager and confirm the newest snapshot is protected.
7. Delete an older test snapshot and verify Version History records the deletion.
8. Preview one retention cleanup without enabling automatic cleanup.
9. Exercise an export if an export folder is configured.
10. Remove the disposable project without deleting source files.
