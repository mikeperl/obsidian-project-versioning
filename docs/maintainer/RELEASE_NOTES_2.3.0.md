# Project Versioning 2.3.0

## New features

- Existing project definitions can now be edited directly from the Command Palette with **Edit versioned project**, in addition to the Settings **Edit** button.
- Changing a project's source folder preserves its project ID, managed snapshot storage, and version history. This supports projects that are moved or renamed.
- Projects can define an optional **Snapshot export folder** using either a vault-relative or absolute desktop path.
- **Export latest snapshot** copies the newest managed ZIP snapshot to the configured export folder.
- **Export chosen snapshot** allows any retained snapshot to be selected and copied to the configured export folder.
- The snapshot manager provides an **Export** button for each snapshot when an export folder is configured.
- Projects may optionally copy every newly created snapshot to the export folder automatically.
- Export folders located inside the project source tree are automatically excluded from subsequent snapshots to prevent recursive ZIP inclusion.

## Compatibility

Existing 2.2.x settings are migrated automatically. Export is disabled unless the user explicitly configures an export folder. Existing archives and reports remain compatible.
