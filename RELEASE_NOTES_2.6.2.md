# Project Versioning 2.6.2 — Release Candidate

This is the planned first Obsidian Community directory submission. It is not a claim of directory acceptance or publication.

## Highlights

- Full or selective local ZIP snapshots for whole vaults, vault folders, and other desktop folders.
- Snapshot comparison with Markdown/JSON reports, persistent version history, and guarded snapshot cleanup.
- Opt-in exclusion of recognized image files and CBR/CBZ/CB7/CBT comic archives.
- Opt-in exclusion of the Obsidian configuration folder from selective snapshots.
- Linked-directory inclusion controls and archive statistics.

## Platform and limitations

- Desktop-only Obsidian plugin; minimum application version declared in the manifest is 1.13.0, subject to compatibility verification.
- Selective ZIP snapshots do not contain omitted source files and are not complete backups.
- Single included source files of 4 GiB or more are unsupported.
- No built-in network upload, telemetry, user account, or shell-command requirement.

## Release correction

- Corrected physical path matching for selective Obsidian configuration exclusion when included directory junctions or aliases reference the configuration folder.
- Added a regression that detects the previous directory-alias bypass. Windows CI and host smoke testing must still pass.

## Release verification still required

Before creating a release, run the clean production build and automated checks, review dependency advisories, and smoke-test the exact compiled release assets inside Obsidian.

## Support

All features are free. [Optional PayPal contributions](https://www.paypal.com/donate/?hosted_button_id=TM6S72Q9PQ93A) support future maintenance.
