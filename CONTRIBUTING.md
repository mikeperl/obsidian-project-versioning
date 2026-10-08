# Contributing

Contributions and issue reports are welcome.

## Before opening an issue

Search existing issues first. Never attach private vault contents or unredacted comparison reports. Reproduce filesystem problems with a disposable test project when possible.

## Development

```bash
npm ci
npm audit
npm run check
```

Pull requests should include tests for changes to the comparison, ZIP, retention, or version-history logic. UI and filesystem changes should also be tested in a disposable desktop Obsidian vault.

## Scope

Project Versioning focuses on understandable local project snapshots and comparison. Features that require accounts, cloud services, telemetry, or unrelated project-management functionality should be proposed before implementation.
