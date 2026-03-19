Follow these steps to create a new release:

## 1. Review unreleased changes

Read `CHANGELOG.md` and show the user all items under `[Unreleased]`. Ask the user to confirm these are the changes they want to include in the release. If the `[Unreleased]` section is empty, stop and inform the user there is nothing to release.

## 2. Suggest version number

Look at the previous version in `CHANGELOG.md` (e.g. `[2.0]`) and analyze the unreleased changes to recommend a version bump:

- **Major** (e.g. `2.0` → `3.0`) — breaking changes, architectural rewrites, or incompatible API changes
- **Minor** (e.g. `2.0` → `2.1`) — new features, enhancements, or non-breaking additions
- **Patch** (e.g. `2.1` → `2.1.1`) — bug fixes only, no new features

Explain the reasoning and let the user confirm or override the suggested version.

## 3. Update project documentation

### CHANGELOG.md

- Rename `## [Unreleased]` to `## [<version>] - <today's date>` (format: `YYYY-MM-DD`)
- Add a new empty `## [Unreleased]` section above it
- Preserve all existing content below

### Other documentation files

Check the project's context file (CLAUDE.md, AGENTS.md, or equivalent) for any project-specific doc paths. Common files to update on release:

- **TODO.md / docs/TODO.md** — mark completed tasks as `[x]`, update phase status labels (e.g. `⬜ TODO` → `✅ COMPLETE`)
- **SPEC.md / docs/SPEC.md** — update acceptance criteria checkboxes, version, and last-updated date if the release changes documented behaviour; skip for purely internal releases

### Version fields

If the project has a `package.json` (or equivalent manifest — `pyproject.toml`, `Cargo.toml`, `build.gradle`, etc.), update the `version` field to the new version. If there are multiple packages or workspaces, update all relevant manifests. Check the project context file for the exact paths.

## 4. Regenerate documentation

If the project has a docs-generation step, run it now. Check the project context file or `package.json` scripts for the command (e.g. `npm run docs:generate`, `make docs`). Skip if no such step exists.

## 5. Run tests

Run the project's test suite to ensure everything passes before releasing. Check the project context file for the test command (e.g. `npm test`, `pytest`, `cargo test`). If tests fail, stop and inform the user.

## 6. Build assets

If the project has a build step, run it now. Check the project context file for the build command(s) (e.g. `npm run build`, `make release`). If the project produces release artifacts (zip files, binaries, dist folders), create them now following the project's convention. Skip if no build step exists.

## 7. Stage, commit, and push

Stage the modified files by name — at minimum `CHANGELOG.md` and any version manifest files updated in step 3. Never use `git add -A`. Never stage files that may contain secrets.

Ask the user which alias to use:

- **`git cai`** — AI-attributed commit (sets a distinct author and prefixes the message with `AI: `)
- **`git ch`** — Regular commit with the user's default git identity

Commit with message: `release: v<version>`

Then push with `git push`.

## 8. Create git tag

```
git tag v<version>
git push --tags
```

## 9. Create GitHub Release

Use `gh release create` to publish the release:

```
gh release create v<version> \
  --title "v<version>" \
  --notes "<changelog entries for this version>"
```

- The `--notes` value should contain the full changelog entries for this version (Added, Changed, Fixed, Removed sections) formatted in markdown.
- If the project produces release artifacts (step 6), attach them by appending their paths to the command.

## 10. Clean up

Remove any temporary build artifacts created in step 6 (e.g. zip archives). Leave the `dist/` or `build/` output in place unless the project convention says otherwise.

Confirm the release is live by showing the GitHub Release URL.
