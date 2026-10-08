# Project Versioning v2.3.17 — Manual Testing Protocol and Evaluation Form

## Test information

| Field | Entry |
|---|---|
| Plugin version | 2.3.17 |
| Test date |  |
| Tester |  |
| Obsidian version |  |
| Operating system |  |
| OS version |  |
| Node.js version |  |
| npm version |  |
| Test vault path |  |
| Source-project path |  |
| Candidate commit/tag |  |
| Candidate artifact SHA-256 |  |

## Evaluation rules

1. Use a dedicated disposable test vault. Do not perform destructive tests against production projects.
2. Perform the tests in the stated order because later tests may depend on data created earlier.
3. Run the protocol against the exact release-candidate `main.js`, `manifest.json`, and `styles.css` intended for distribution.
4. A test passes only when **every** expected result occurs.
5. Mark exactly one result for every required test by checking either **PASS** or **FAIL**.
6. If a test cannot be completed, encounters an unexpected condition, produces an error, or differs from the expected behavior, mark **FAIL** and document the reason.
7. Do not correct unexpected data or configuration during a test without recording what happened.
8. Record screenshots, console output, or exact error text for every failure where practical.
9. After a failure, continue testing only where doing so cannot damage data or invalidate later results.
10. After fixing a defect, rerun the failed test plus logically related tests and record the retest in the failure notes or Failure Record.
11. For filesystem-failure tests, preserve unexpected files/folders for inspection. The plugin must prefer refusing an operation over deleting or overwriting data it cannot prove it owns.
12. Optional platform-specific tests may be skipped only when clearly marked optional and not applicable to the test platform.

## Test environment

Prepare:

- a disposable Obsidian vault named `PV-Test-Vault`;
- folders `Project-A`, `Project-B`, and `Project-C`;
- an external test folder outside the vault;
- an external storage folder outside the vault;
- an export folder outside the vault;
- several small text files, one binary file, and one larger text file;
- a copy of the exact release-candidate plugin files.

> [!IMPORTANT]
> Perform destructive tests only in disposable test folders and vaults. Never use irreplaceable project data for this protocol.

---

# Phase 1 — Clean Test Environment

## Test 1.1 — Disposable vault

### Instructions

1. Create or open `PV-Test-Vault`.
2. Confirm the vault contains no prior `.project-versions` directory and no old Project Versioning settings.
3. Confirm test source, storage, and export folders are disposable.

### Expected result

testing begins from a known clean state.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 1.2 — Candidate files

### Instructions

1. Install the exact candidate `main.js`, `manifest.json`, and `styles.css` into `.obsidian/plugins/project-versioning/`.
2. Confirm all three files belong to the same candidate version.

### Expected result

no mixed-version installation.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 2 — Plugin Startup and Enablement

## Test 2.1 — Enable plugin

### Instructions

1. Reload Obsidian.
2. Enable Project Versioning.
3. Open Developer Console.

### Expected result

plugin enables without uncaught exception or startup error.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 2.2 — Disable and re-enable

### Instructions

1. Disable the plugin.
2. Re-enable it.

### Expected result

clean unload/reload; no duplicated ribbon icons, commands, settings, or errors.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 3 — Settings, Commands, and Ribbon

## Test 3.1 — Settings page

### Instructions

Open **Settings → Project Versioning**.

### Expected result

page renders correctly with configured-project, comparison, storage/privacy, and retained-data sections as applicable. In a clean vault with no managed archives or reports, **Delete stored data...** is disabled.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 3.2 — Command registration

### Instructions

Open the Command Palette and verify these commands are present:

- Archive and compare project
- Manage snapshots
- Compare latest snapshots
- Compare chosen snapshots
- Open latest comparison report
- Open version history
- Add versioned project
- Edit versioned project
- Export latest snapshot
- Export chosen snapshot

### Expected result

all commands appear once and no default hotkeys are assigned.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 3.3 — Ribbon

### Instructions

Use the archive ribbon action.

### Expected result

it invokes the archive workflow and does not duplicate after reload.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 4 — Add a Whole-Vault Project

## Test 4.1 — Add vault root

### Instructions

1. Add a project.
2. Select the entire vault as source (`.`).
3. Leave managed storage on Default.
4. Save.

### Expected result

the unsaved Add Project form accepts Default storage before a permanent internal project ID exists; no internal-identity validation error appears. After Save, the project receives a valid ID and default storage resolves under `.project-versions/<project-id>`.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 4.2 — Internal storage exclusion

### Instructions

Inspect the source and later snapshot contents.

### Expected result

`.project-versions` is never included in whole-vault snapshots.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 5 — Add a Vault-Subfolder Project

## Test 5.1 — Vault-relative source

### Instructions

Add `Project-A` as a separate project using a vault-relative path.

### Expected result

source is stored/displayed vault-relative and resolves correctly.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 5.2 — Duplicate active source

### Instructions

Attempt to add another active project pointing to the same physical source.

### Expected result

save is blocked with a clear validation message.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 6 — Add an External-Folder Project

## Test 6.1 — Absolute source

### Instructions

Add a disposable folder outside the vault.

### Expected result

absolute desktop path is accepted and remains outside the vault.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 6.2 — Source existence

### Instructions

Enter a nonexistent source path.

### Expected result

save is blocked; the plugin does not silently create the project source.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 7 — Folder Entry, Autocomplete, and Selectors

## Test 7.1 — Path autocomplete

### Instructions

Type source/storage/export paths segment by segment.

### Expected result

valid matching folders are suggested; accepting a suggestion produces the intended path.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 7.2 — Vault selector

### Instructions

Use **Vault...** to choose a vault folder.

### Expected result

the stored value is vault-relative.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 7.3 — Filesystem browser

### Instructions

Use **Browse...** to navigate drives/folders, parent navigation, and **Use current folder**.

### Expected result

current path is visible; inaccessible locations cannot be selected; selecting a folder inside the vault stores a vault-relative path.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 7.4 — Open folder

### Instructions

Use **Open** for an existing source/storage/export folder.

### Expected result

the operating-system file manager opens the intended location.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 8 — Project Name and Filename Prefix Validation

## Test 8.1 — Valid names

### Instructions

Test ordinary spaces, hyphens, underscores, brackets, Unicode, `@`, `#`, and `&`.

### Expected result

valid names save normally.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 8.2 — Invalid names

### Instructions

Try reserved characters, control characters, leading/trailing whitespace, trailing periods, `.`/`..`, and Windows device names such as `CON`, `NUL`, `COM1`, and `LPT1`.

### Expected result

save is blocked with clear validation.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 8.3 — Explicit prefix

### Instructions

Set a valid explicit archive filename prefix.

### Expected result

new snapshots use the prefix exactly.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 9 — Managed Storage Validation

## Test 9.1 — Default storage

### Instructions

Leave Storage folder blank.

### Expected result

stable private storage is used for the project.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 9.2 — Valid custom storage

### Instructions

Configure an empty external storage folder.

### Expected result

project saves and managed archives/reports use that folder.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 9.3 — Source/storage overlap

### Instructions

Try storage equal to, inside, or containing the source.

### Expected result

all unsafe relationships are rejected.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 9.4 — Cross-project overlap

### Instructions

Try storage overlapping another active or retained project's managed storage or active source.

### Expected result

save is blocked.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 10 — Snapshot Export Configuration

## Test 10.1 — Configure export folder

### Instructions

Set a valid export folder.

### Expected result

manual export becomes available and the automatic-copy toggle is shown.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 10.2 — Clear export folder

### Instructions

Clear the export path.

### Expected result

automatic-copy option is hidden/disabled and no stale export configuration is used.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 10.3 — Export inside source

### Instructions

Configure an export folder inside the project source.

### Expected result

configuration is allowed only with automatic exclusion from snapshot contents; exported ZIPs do not recursively enter later snapshots.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 10.4 — Export/managed-storage overlap

### Instructions

Try export inside managed storage or another prohibited managed relationship.

### Expected result

unsafe configuration is rejected.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 11 — First Snapshot Baseline

## Test 11.1 — Create baseline

### Instructions

Run **Archive and compare project** for a project with no snapshots.

### Expected result

one valid ZIP is created; it becomes the baseline; no false comparison against nonexistent history occurs.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 11.2 — ZIP contents

### Instructions

Open the ZIP externally.

### Expected result

expected project files are present; managed storage and configured exclusions are absent.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 11.3 — Version History

### Instructions

Open Version History.

### Expected result

baseline snapshot appears with correct filename/status/hash metadata.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 12 — Unchanged Repeated Snapshot

## Test 12.1 — Create unchanged snapshot

### Instructions

Without modifying source files, create another snapshot.

### Expected result

a new snapshot is created and comparison correctly reports no project-content change.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 12.2 — Comparison artifacts

### Instructions

Inspect Markdown/JSON reports.

### Expected result

both describe the same snapshot pair and agree on the result.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 13 — Added Files

## Test 13.1 — Add text file

### Instructions

Add a new text file and snapshot.

### Expected result

report classifies it as added.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 13.2 — Add nested folder/file

### Instructions

Add a nested directory containing a file.

### Expected result

nested path is archived and reported correctly.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 14 — Modified Text and Line Diffs

## Test 14.1 — Ordinary edit

### Instructions

Modify several lines in a text file.

### Expected result

file is classified as modified and a unified diff is rendered.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 14.2 — Diff context

### Instructions

Change global diff-context setting and repeat.

### Expected result

rendered context changes accordingly without changing file classification.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 14.3 — Render limit

### Instructions

Force a diff large enough to exceed the rendered-line limit.

### Expected result

report truncation/summary behavior is clear and does not lose the fact that the file changed.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 15 — Deleted Files

## Test 15.1 — Delete file

### Instructions

Delete a previously snapshotted file and snapshot again.

### Expected result

report classifies it as deleted.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 15.2 — Delete nested content

### Instructions

Delete a nested file/folder tree.

### Expected result

deletions are represented accurately without phantom additions.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 16 — Rename and Move Detection

## Test 16.1 — Rename unchanged file

### Instructions

Rename a file without changing contents.

### Expected result

report identifies rename/move rather than unrelated delete+add when similarity rules permit.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 16.2 — Move unchanged file

### Instructions

Move a file to another folder.

### Expected result

move is identified correctly.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 16.3 — Rename plus edit

### Instructions

Rename a text file and modify it moderately.

### Expected result

similarity threshold behavior is reasonable and deterministic.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 17 — Binary and Large Text Files

## Test 17.1 — Binary modification

### Instructions

Change a binary file.

### Expected result

change is detected by size/hash without meaningless text diff.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 17.2 — Oversized text

### Instructions

Modify a text file larger than the configured text-diff size limit.

### Expected result

change is still detected and hashed; line-by-line diff is intentionally omitted.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 18 — Extra Snapshot Exclusions

## Test 18.1 — Exclusion glob

### Instructions

Configure an exclusion such as `temp/**`, create matching files, and snapshot.

### Expected result

excluded files are absent from the ZIP and reports.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 18.2 — Nonmatching files

### Instructions

Create nearby files that should not match the exclusion.

### Expected result

they remain versioned.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 19 — Metadata-Only and Generated/Derived Classification

## Test 19.1 — Metadata-only pattern

### Instructions

Configure metadata-only regex patterns and change only matching lines.

### Expected result

file is classified metadata-only.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 19.2 — Mixed substantive change

### Instructions

Change both a matching metadata line and substantive content.

### Expected result

file is not incorrectly reduced to metadata-only.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 19.3 — Generated path

### Instructions

Configure a generated/derived path and modify a matching file.

### Expected result

change remains recorded but is classified generated/derived.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 20 — Multiple Projects and Project Picker

## Test 20.1 — Multiple active projects

### Instructions

Configure at least three distinct projects.

### Expected result

commands requiring a project display a searchable picker.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 20.2 — Correct routing

### Instructions

Run archive/manage/export operations on each project.

### Expected result

each action uses only the selected project's source/storage/history.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 21 — Snapshot Manager Display

## Test 21.1 — Snapshot listing

### Instructions

Open **Manage snapshots**.

### Expected result

snapshots display filename/date/size/status accurately and total storage is plausible.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 21.2 — Newest protection

### Instructions

Inspect deletion controls.

### Expected result

newest valid snapshot cannot be manually selected for deletion.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 21.3 — Malformed status placeholder

### Instructions

If malformed-state testing has already been performed, confirm malformed archives remain visible for inspection/deletion but are not treated as valid lineage.

### Expected result

Malformed archives remain visible for inspection/deletion but are not treated as valid lineage.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 22 — Manual Snapshot Deletion

## Test 22.1 — Delete older snapshot

### Instructions

Select an older snapshot and confirm deletion.

### Expected result

intended archive is removed; source project is untouched.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 22.2 — Related reports/history

### Instructions

Inspect reports and Version History.

### Expected result

reports referencing the deleted snapshot are removed as appropriate; historical record remains marked as deleted rather than silently erased.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 22.3 — Cancel deletion

### Instructions

Start deletion and cancel confirmation.

### Expected result

nothing changes.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 23 — Consecutive Duplicate Removal

## Test 23.1 — Consecutive identical snapshots

### Instructions

Create a run of unchanged snapshots and use **Remove duplicates**.

### Expected result

older redundant copies in each unchanged run are proposed/removed while the newest copy of the run is preserved.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 23.2 — Non-consecutive same state

### Instructions

Change the project, then later restore an earlier project state.

### Expected result

non-consecutive historically distinct snapshot is not removed merely because content equals an older state.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 24 — Retention Policies

## Test 24.0 — Retention progressive disclosure

### Instructions

1. Open **Add project** or **Edit project** with retention disabled.
2. Verify only **Enable retention policy** is visible in the retention section.
3. Enable retention.
4. Select each retention mode in turn.

### Expected result

- With retention disabled, **Retention mode**, all mode-specific numeric fields, and **Clean up automatically after new snapshot** are hidden.
- With retention enabled, **Retention mode** and **Clean up automatically after new snapshot** are visible.
- **Keep everything** shows no numeric retention fields.
- **Keep latest n snapshots** shows only **Latest snapshots to keep**.
- **Keep snapshots from last n days** shows only **Retention age in days**.
- **Tiered: recent + daily + weekly** shows only **Tiered recent snapshots** and **Tiered daily window**.
- Switching modes updates the visible controls immediately without closing the dialog.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 24.1 — Keep everything

### Instructions

Preview/apply.

### Expected result

no snapshot deletion.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 24.2 — Keep latest N

### Instructions

Configure a small N and preview/apply.

### Expected result

correct older snapshots are selected; newest remains protected.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 24.3 — Last N days

### Instructions

Use test snapshots/dates sufficient to exercise the rule.

### Expected result

age-based selection matches configuration while newest remains protected.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 24.4 — Tiered retention

### Instructions

Exercise recent + daily + weekly retention.

### Expected result

preview and actual cleanup agree.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 24.5 — Automatic cleanup

### Instructions

Enable automatic cleanup and create a new snapshot.

### Expected result

cleanup occurs only after successful snapshot creation and respects the configured policy.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 25 — Renaming in an Existing Versioned Project

## Initial state

Use the established Phase 25 baseline:

- project name: `Project-B`;
- source folder already versioned;
- existing snapshots: V1 and V2;
- original archive prefix: `Project B`.

## Test 25.1 — Rename project only

### Instructions

1. Edit `Project-B`.
2. Change the project name to `Project-B-Renamed`.
3. Do not change the source folder.
4. Save.

### Expected result

project is renamed, not treated as moved or recreated; existing history remains attached to the same project identity.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 25.2 — Snapshot after project rename

### Instructions

Create another snapshot.

### Expected result

V1/V2 remain recognized as prior history and versioning continues rather than starting a new lineage.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 25.3 — Change archive prefix only

### Instructions

1. Edit `Project-B-Renamed`.
2. Change only the archive filename prefix.
3. Save and create another snapshot.

### Expected result

old snapshots keep their old filenames; new snapshot uses the new prefix; old and new prefixes remain part of one continuous project history.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 25.4 — History and comparison continuity

### Instructions

Open Snapshot Manager, Version History, and comparison commands.

### Expected result

snapshots created before and after rename/prefix change remain discoverable, comparable, and correctly ordered.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 26 — Project Editing and Dirty-State Behavior

## Test 26.1 — Save disabled without changes

### Instructions

Open an existing project and make no effective change.

### Expected result

**Save project** is disabled; no misleading “changes saved” message occurs.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 26.2 — Change then revert

### Instructions

Modify a field, then restore its original value.

### Expected result

Save becomes enabled after the effective change and disabled again after exact reversion.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 26.3 — Unsaved-close protection

### Instructions

1. Make an effective change.
2. Focus a path field so its autocomplete is open, then press **Escape** once.
3. Confirm only the autocomplete closes.
4. Close the editor with the title-bar **X**.
5. Choose **Continue editing**.
6. Close again and choose **Discard changes**.

### Expected result

the first Escape closes only the open autocomplete; the editor remains interactive. The title-bar X always initiates normal close behavior rather than being consumed by autocomplete state. Discard confirmation appears, **Continue editing** returns to the editor, and **Discard changes** closes it. The editor never becomes stuck or impossible to close.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 26.4 — Validation blocks Save

### Instructions

Enter an invalid path/name.

### Expected result

Save remains blocked even though the form is dirty.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 27 — Persistence Across Restart

## Test 27.1 — Settings persistence

### Instructions

1. Configure multiple projects and nondefault options.
2. Fully close Obsidian.
3. Reopen the vault.

### Expected result

project definitions, storage/export settings, exclusions, comparison settings, and retention configuration persist.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 27.2 — Snapshot numbering after restart

### Instructions

Create a snapshot after restart.

### Expected result

numbering/history continues from the existing lineage; no V1 reset or filename reuse occurs.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 27.3 — Immediate settings refresh

### Instructions

Add/edit/remove a project and inspect Settings without restarting.

### Expected result

the settings UI reflects the committed change immediately.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 28 — Source Folder Move/Rename

## Test 28.1 — Move source folder externally

### Instructions

Close or pause operations, move/rename the project source folder in the OS, then edit the project to the new path.

### Expected result

project ID, managed storage, and existing history are preserved; the editor warns about source-folder continuity where appropriate.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 28.2 — Snapshot after source-path change

### Instructions

Create a snapshot from the updated source.

### Expected result

operation is explicit and safe; history is not silently redirected through an invalid or stale path.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 29 — Managed Storage Migration

## Test 29.1 — Default to custom storage

### Instructions

Move an existing project with history from Default to an empty custom destination.

### Expected result

archives/reports migrate and verify before settings commit; old history remains usable.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 29.2 — Custom to Default

### Instructions

Return the same project to Default.

### Expected result

it returns to the stable `.project-versions/<project-id>` identity rather than creating a numbered replacement location.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 29.3 — Non-empty destination

### Instructions

Try migrating into a non-empty unrelated destination.

### Expected result

migration is rejected without merging or overwriting unrelated data.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 29.4 — Unrelated file in old custom storage

### Instructions

Place an unrelated file beside managed `archives/` and `reports/`, then migrate elsewhere.

### Expected result

only managed trees move; unrelated file remains in the original location.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 30 — Offline Operation and Privacy

## Test 30.1 — Offline core operation

### Instructions

Disconnect networking and use normal archive, compare, manage, history, and export functions.

### Expected result

core functionality continues without Internet access.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 30.2 — Network observation

### Instructions

Observe Developer Tools/network activity while using the plugin.

### Expected result

Project Versioning initiates no telemetry, analytics, update checks, uploads, or other network requests.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 30.3 — External execution

### Instructions

Review visible behavior and release audit results.

### Expected result

plugin does not invoke Python, shell commands, Git, or external diff programs for normal operation.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 31 — Manual and Automatic Snapshot Export

## Test 31.1 — Export latest

### Instructions

Run **Export latest snapshot**.

### Expected result

latest valid managed snapshot is copied to the configured export folder without changing the managed archive.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 31.2 — Export chosen

### Instructions

Run **Export chosen snapshot** and select an older valid snapshot.

### Expected result

selected snapshot is copied accurately.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 31.3 — Automatic export

### Instructions

Enable **Copy each new snapshot to export folder** and create a snapshot.

### Expected result

managed snapshot is created first and an identical export copy appears.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 31.4 — No overwrite

### Instructions

Repeat when a same-named file already exists in export.

### Expected result

unrelated existing file is never overwritten.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 32 — Comparison Commands, Reports, and Version History

## Test 32.1 — Compare latest snapshots

### Instructions

Run the command on a project with at least two valid snapshots.

### Expected result

correct latest valid pair is compared.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 32.2 — Compare chosen snapshots

### Instructions

Choose two older valid snapshots.

### Expected result

selected identities are compared, independent of current filename prefix.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 32.3 — Latest stable report

### Instructions

With stable latest reports enabled, run a comparison and open the latest report.

### Expected result

Markdown and JSON correspond to the same pair; stale/mismatched pairs are not presented as valid.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 32.4 — Version History

### Instructions

Inspect history after create/delete/rename/prefix-change operations.

### Expected result

statuses and snapshot identities remain coherent.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 33 — Remove Project and Retained History

## Test 33.1 — Remove only

### Instructions

Remove a project using **Remove only**.

### Expected result

active project definition disappears; managed snapshots/reports remain; source folder remains untouched; retained data is shown in Settings.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 33.2 — Re-add same source with default storage

### Instructions

Re-add the removed source.

### Expected result

retained default lineage reconnects to the same project identity/history rather than creating a new numbered storage folder.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 33.3 — Delete retained data

### Instructions

Remove again while keeping data, then delete the retained versioning data from Settings.

### Expected result

only plugin-managed `archives/` and `reports/` are removed; source and unrelated files remain.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 34 — Remove + Delete Data and Global Cleanup

## Test 34.1 — Remove + delete data

### Instructions

Use **Remove + delete data** on a disposable project.

### Expected result

managed archives/reports are deleted and project definition is removed; source folder is untouched.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 34.2 — Export copies

### Instructions

If export copies exist, verify the deletion confirmation/behavior matches the UI and safety rules.

### Expected result

only verifiably owned matching exports are removed when explicitly requested; conflicts are preserved and reported.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 34.3 — Delete all stored versioning data

### Instructions

Use the global cleanup action with multiple configured/retained projects.

### Expected result

managed versioning data is removed according to confirmation; failed/conflicting cleanup does not cause a later orphan sweep to bypass safety protections. After successful cleanup removes the last managed archive/report, Settings refreshes and **Delete stored data...** is disabled. If new managed data is later created, the action becomes enabled again after Settings refresh.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 35 — Invalid, Missing, and Unavailable Paths

## Test 35.1 — Source disappears

### Instructions

Temporarily rename/remove a configured source folder and invoke archive.

### Expected result

clear failure; no successful snapshot/history entry is created.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 35.2 — Storage unavailable

### Instructions

Make custom storage unavailable or read-only where practical.

### Expected result

operation fails safely; existing history remains intact.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 35.3 — Export unavailable

### Instructions

Make export unavailable while managed storage remains available.

### Expected result

managed snapshot remains valid; export failure is reported separately and does not invalidate the managed snapshot.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 35.4 — Invalid persisted configuration

### Instructions

Using only a disposable vault, manually damage a project path in plugin data and restart.

### Expected result

invalid configuration is not blindly trusted for filesystem operations.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 36 — Uninstall/Reinstall and Data Boundaries

## Test 36.1 — Disable/uninstall semantics

### Instructions

Disable/remove plugin files without explicitly deleting stored versioning data.

### Expected result

source project and stored snapshots remain ordinary filesystem data; plugin does not erase them during unload/uninstall.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 36.2 — Reinstall candidate

### Instructions

Restore the plugin and reload.

### Expected result

persisted configuration/history remains usable when compatible.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 36.3 — Source immutability

### Instructions

Across all removal/cleanup operations, verify source project files were never modified or deleted by versioning-data cleanup.

### Expected result

No removal, cleanup, migration, retention, or uninstall operation modifies or deletes project source files.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 37 — Final Release-Candidate Regression

## Test 37.1 — Realistic end-to-end project

### Instructions

Using a disposable but realistic project:
1. add project;
2. create baseline;
3. add/modify/delete/rename files;
4. create further snapshots;
5. compare;
6. inspect reports/history;
7. export;
8. manage/delete/retain snapshots;
9. edit project;
10. restart Obsidian and repeat a snapshot.

### Expected result

complete workflow is internally consistent and understandable without developer intervention.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 37.2 — Automated release checks

### Instructions

Run the repository's full automated check in the supported development environment.

### Expected result

tests, audit, lint, and production build pass for the candidate.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 37.3 — Distribution files

### Instructions

Confirm `main.js`, `manifest.json`, and `styles.css` are the matching candidate set and that version numbers are synchronized.

### Expected result

candidate is suitable to proceed to release preparation if Phase 38 also passes.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Phase 38 — Failure-Safety and Corrupted-State Testing

This phase validates the user-visible consequences of the filesystem and transaction hardening added after the original protocol. Use disposable data and retain backups of any snapshot you intentionally corrupt.

## Test 38.1 — Filesystem-root rejection

### Instructions

1. Open **Add project**.
2. In the filesystem browser navigate to a filesystem root such as `C:\`, `D:\`, `/`, or an external mounted-volume root.
3. Try **Use current folder**.
4. Try entering the same root manually.
5. Repeat while editing an existing project.

### Expected result

no route can save or execute a project whose source is an entire filesystem/mounted-volume root. The UI provides a clear blocking validation result.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.2 — Unsafe storage relationships

### Instructions

Attempt each of the following with disposable paths:

- storage equal to source;
- storage inside source;
- storage containing source;
- export inside managed storage;
- custom storage overlapping another active source/storage tree.

### Expected result

unsafe relationships are rejected before save and again at operation time if configuration has been tampered with.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.3 — Storage migration with unrelated files

### Instructions

1. Use custom storage containing valid managed `archives/` and `reports/` plus an unrelated file such as `DO-NOT-MOVE.txt`.
2. Migrate managed storage to a new empty folder.

### Expected result

managed archives/reports move and remain usable; `DO-NOT-MOVE.txt` stays in the original folder unchanged.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.4 — Source-folder change on an existing project

### Instructions

1. Use a project with established history.
2. Edit its source to a genuinely different folder.
3. Observe the warning.
4. Save and create a snapshot only after acknowledging the intended change.

### Expected result

the source-history continuity risk is made explicit; project identity/storage are not silently recreated; operation uses the newly saved source only.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.5 — Missing managed snapshot

### Instructions

1. Create at least three snapshots.
2. Close Obsidian.
3. Manually move one older ZIP out of `archives/`.
4. Reopen Obsidian and inspect Snapshot Manager and Version History.
5. Create another snapshot.

### Expected result

history marks the absent archive `Missing`; remaining valid snapshots function normally; the missing historical filename/version is not silently reused.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.6 — Malformed managed snapshot

### Instructions

1. Back up an older managed snapshot ZIP.
2. Replace that ZIP with a plain-text file using the same `.zip` filename.
3. Reopen/refresh Snapshot Manager and Version History.
4. Try comparison/export actions.

### Expected result

malformed file remains visible for inspection/manual deletion; history shows `Malformed`; it is not used as an automatic predecessor, latest export target, duplicate candidate, or retention-protected valid latest snapshot; valid snapshots remain usable.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.7 — Export collision protection

### Instructions

1. Export a snapshot.
2. Replace the exported copy with a different same-named file.
3. Export the same snapshot again.

### Expected result

existing different file is preserved; plugin refuses to overwrite it and reports the conflict.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.8 — Export-cleanup conflict

### Instructions

1. Configure snapshot export and create exported snapshots.
2. Replace or modify one exported ZIP while retaining the same filename.
3. Invoke a project/data removal path that offers export cleanup.

### Expected result

conflicting external file is preserved; unsafe cleanup is reported; managed history is not silently deleted through a secondary/orphan cleanup path merely because export cleanup conflicted.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.9 — Deleted-version numbering

### Instructions

1. On the same day create V1, V2, and V3.
2. Delete V3 through the plugin.
3. Create another snapshot.
4. Repeat in a separate disposable project by deleting a snapshot manually outside Obsidian before creating the next snapshot.

### Expected result

historical snapshot identities are not recycled. A deleted or missing version number already recorded in history remains reserved.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.10 — Interrupted or unavailable external storage

### Instructions

1. Use external managed storage or export storage on a removable/network location.
2. Make the location unavailable before an operation.
3. Attempt archive/export/cleanup as applicable.
4. Restore the location and retry.

### Expected result

unavailable storage produces a clear safe failure; no false successful history entry or partial final-path snapshot appears; existing history remains intact; normal operation resumes after the storage returns.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

## Test 38.11 — Windows junction/symlink containment (optional)

### Instructions

1. In a disposable Windows project source, create a directory junction or symlink pointing to a folder outside the project.
2. Put a recognizable file in the external target.
3. Create a snapshot.

### Expected result

linked external content is not traversed into the snapshot. If the project source itself resolves through an unsafe alias/root relationship, the operation is rejected rather than following it.

### Result

- [ ] PASS
- [ ] FAIL

**Failure notes:**

---

# Final Pass Criteria

The release candidate passes manual validation only when:

- every required test is marked PASS;
- every failure has either been fixed and retested or explicitly classified as an accepted release limitation;
- no destructive operation has modified the source project or unrelated user files;
- no malformed or partial snapshot is represented as successful;
- no unsafe filesystem root or overlapping managed-storage relationship can reach snapshot execution;
- restart persistence and history continuity are correct;
- automated release checks pass on the exact candidate build.

# Final Regression Review

Before accepting the release, confirm all previously discovered problem areas remained corrected during the complete test:

- [ ] Project names and valid archive prefixes are preserved exactly.
- [ ] Invalid filename characters cannot reach snapshot filenames.
- [ ] Blank archive prefix follows the current project name as designed.
- [ ] Project/default-storage identity remains stable across storage reassignment.
- [ ] Returning to Default does not create an unnecessary numbered storage identity.
- [ ] Removed-but-retained history does not masquerade as an active project.
- [ ] Re-adding the same source reconnects retained lineage where applicable.
- [ ] Latest valid snapshot remains protected from destructive retention/deletion.
- [ ] Version History correctly distinguishes Available, Missing, Malformed, and deliberate Deleted states.
- [ ] Hidden/internal/system folders and filesystem roots cannot be selected where unsafe.
- [ ] Source and managed-storage trees cannot overlap unsafely.
- [ ] Managed-storage trees cannot overlap each other unsafely.
- [ ] Export collision protection never overwrites an unrelated same-named file.
- [ ] Cleanup never deletes project source files.
- [ ] Cleanup and migration preserve unrelated files and replacement filesystem objects.
- [ ] Existing snapshots are never silently overwritten or filename identities silently reused.
- [ ] Malformed snapshots cannot become automatic lineage, retention, comparison, or export targets.
- [ ] Interrupted or unavailable storage fails safely without false success state.
- [ ] Latest report Markdown/JSON pairs are internally consistent before being opened.

# Failure Record

Duplicate this section for every failure that needs a durable defect record.

## Failure ID: `PV-MANUAL-____`

- **Phase/test:**
- **Plugin version:**
- **Environment:**
- **Initial state:**
- **Steps to reproduce:**
- **Expected result:**
- **Actual result:**
- **Data affected:**
- **Was source project data modified?** Yes / No
- **Was unrelated filesystem data modified?** Yes / No
- **Console error/log:**
- **Screenshot/file evidence:**
- **Probable severity:** Critical / High / Medium / Low
- **Fix reference:**
- **Automated regression added:** Yes / No / N/A
- **Targeted tests rerun:**
- **Retest status:**
- **Remaining uncertainty:**

# Final Evaluation

## Failure count

**Number of failed tests:**  

## Defect references

1.
2.
3.
4.
5.

## Release decision

Mark PASS only when every required test above passes and no unresolved defect could cause data loss, corrupted history, incorrect comparison results, unsafe deletion, or materially misleading behavior.

- [ ] PASS
- [ ] FAIL

## Release suitability

- **Personal use:** [ ] ACCEPT  [ ] DO NOT ACCEPT
- **Beta testing:** [ ] ACCEPT  [ ] DO NOT ACCEPT
- **Public release:** [ ] ACCEPT  [ ] DO NOT ACCEPT

## Tester comments

**Overall assessment:**  

**Remaining concerns:**  

**Recommended next action:**
