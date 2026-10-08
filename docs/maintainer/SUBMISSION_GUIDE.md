# GitHub and Obsidian Community submission guide

This repository is prepared to become the public source repository for **Project Versioning 2.3.17**.

## 1. Verify the release candidate locally

From a fresh checkout on the Windows development machine, run:

```bash
npm ci
npm audit
npm run check
```

All commands must pass. `npm run check` executes the core tests, repository submission audit, ESLint, TypeScript checking, and the minified production build.

Then install the generated `main.js`, `manifest.json`, and `styles.css` in a disposable desktop Obsidian vault and execute the applicable final manual tests.

## 2. Create the public GitHub repository

Create a public repository, preferably named:

```text
project-versioning
```

Do not initialize it with another README, license, or `.gitignore`; this source package already contains them. Configure the Git author name and public/noreply email you want exposed before the initial commit.

From the extracted clean source folder:

```bash
git init
git add .
git commit -m "Initial public release of Project Versioning"
git branch -M main
git remote add origin <YOUR-GITHUB-REPOSITORY-URL>
git push -u origin main
```

Wait for CI to pass on both Ubuntu and Windows before creating a release tag.

## 3. Create the release tag

For release 2.3.17:

```bash
git tag -a 2.3.17 -m "2.3.17"
git push origin 2.3.17
```

The tag must exactly match `manifest.json` and `package.json`, without a `v` prefix.

## 4. Inspect and publish the GitHub release

The release workflow:

- performs `npm ci`, `npm audit`, and `npm run check`;
- verifies that the Git tag matches the manifest version;
- generates build-provenance attestations for `main.js`, `manifest.json`, and `styles.css`;
- creates a **draft** GitHub release with those three files attached.

Inspect the draft, replace or edit generated notes as desired using `docs/maintainer/RELEASE_NOTES_2.3.17.md`, then publish it.

Download the three assets from the published release and install those exact files into the disposable Obsidian vault for the final release-asset smoke test.

## 5. Submit to the Obsidian Community directory

1. Sign in to the Obsidian Community directory with the Obsidian account.
2. Connect the GitHub account to the Community profile.
3. Add a new plugin and supply the public GitHub repository.
4. Confirm the developer-policy attestations and submit.
5. Review automated feedback. If a source or release correction is required, increment the plugin version and publish a new matching GitHub release rather than replacing the existing release.

Only the initial Community directory submission is required. Later plugin versions are distributed from matching GitHub releases.

## 6. Beta coverage

Windows is the primary tested platform. Beta coverage on macOS and Linux is still desirable because the plugin supports desktop Obsidian on all three platforms. Use disposable or backed-up projects when testing filesystem, retention, cleanup, migration, and export behavior.

## Official references

- Obsidian plugin submission: https://docs.obsidian.md/plugins/releasing/submit-plugin
- Plugin submission requirements: https://docs.obsidian.md/community-directory/submission-requirements-for-plugins
- Developer policies: https://docs.obsidian.md/community-directory/developer-policies
- GitHub Actions release workflow: https://github.com/obsidianmd/obsidian-developer-docs/blob/main/en/Plugins/Releasing/Release%20your%20plugin%20with%20GitHub%20Actions.md
