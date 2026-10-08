# Development

## Architecture

```text
src/
├── main.ts
├── settings.ts
├── types.ts
├── electron.d.ts
├── core/
│   ├── cleanup.ts
│   ├── compare.ts
│   ├── crc32.ts
│   ├── diff.ts
│   ├── drive-list.ts
│   ├── filename-policy.ts
│   ├── folder-policy.ts
│   ├── folder-suggestions.ts
│   ├── intraline-diff.ts
│   ├── open-folder.ts
│   ├── path-utils.ts
│   ├── project-edit-state.ts
│   ├── project-identity.ts
│   ├── project-order.ts
│   ├── project-validation.ts
│   ├── snapshot-feedback.ts
│   ├── snapshots.ts
│   ├── storage-migration.ts
│   ├── version-history.ts
│   ├── versioner.ts
│   └── zip.ts
└── ui/
    ├── project-modal.ts
    ├── project-picker.ts
    ├── report-modal.ts
    ├── snapshot-manager.ts
    ├── snapshot-picker.ts
    └── version-history-modal.ts
```

The core versioning engine intentionally does not depend on Obsidian APIs. `main.ts` supplies Obsidian-specific runtime information such as the vault base path and configuration-directory exclusions.

## Local setup

Node.js 20 or later is required. The repository includes `package-lock.json`, so use a reproducible clean install:

```bash
npm ci
```

For a complete local verification:

```bash
npm audit
npm run check
```

`npm audit` checks the installed dependency tree for known vulnerabilities. `npm run check` runs the core tests, repository submission audit, Obsidian-oriented ESLint rules, TypeScript checking, and a minified production build.

For active development:

```bash
npm run dev
```

## Release build

```bash
npm run build
```

Production output is a minified `main.js`. `main.js` is intentionally ignored by Git and belongs in GitHub release assets, not in the source repository.

## Release process

1. Update `CHANGELOG.md`.
2. Update the version in `package.json` and `manifest.json` using semantic versioning.
3. Keep `versions.json` consistent with the minimum supported Obsidian version.
4. Run `npm audit`, then `npm run check`.
5. Commit and push the source.
6. Wait for ordinary branch CI to pass on Ubuntu and Windows.
7. Create and push an annotated tag whose text exactly matches the manifest version, for example `git tag -a 2.3.17 -m "2.3.17"`.
8. The release workflow rebuilds and verifies the plugin, generates build-provenance attestations for the release assets, and creates a draft GitHub release containing `main.js`, `manifest.json`, and `styles.css`.
9. Inspect the draft release, edit its notes as needed, and publish it manually.
10. Install the assets downloaded from the published GitHub release in a disposable Obsidian vault for a final smoke test.

The release tag must be the exact version, with no `v` prefix.
