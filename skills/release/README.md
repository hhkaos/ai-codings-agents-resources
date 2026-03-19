# Skill: `release`

Guides you through creating a new versioned release: CHANGELOG update, version bump, documentation sync, tests, optional build, git tag, and GitHub Release — all in one flow.

**What it does:**
1. Shows unreleased CHANGELOG entries and confirms the scope
2. Suggests a semver bump (major / minor / patch) with reasoning
3. Updates `CHANGELOG.md`, version manifests, and optional doc files
4. Runs docs generation and tests
5. Builds and packages release artifacts (if applicable)
6. Commits and pushes (via `git ch` or `git cai`)
7. Creates a git tag and a GitHub Release via `gh`

**When to use it:** Any time you're ready to cut a release and want the full ceremony handled automatically.

> Requires `git ch` and `git cai` aliases and the `gh` CLI to be configured. See the [new project checklist](../../guides/new-project-checklist.md#3-git-configuration) for alias setup.

---

## Install

→ See [How to install a skill](../README.md#how-to-install-a-skill) for per-agent file paths, front matter, and the quick-install prompt.

**Suggested description for front matter:**
> Version bump, CHANGELOG update, git tag, and GitHub Release in one guided flow.

### Customising for your project

The skill adapts to your project by reading your context file (CLAUDE.md, AGENTS.md, or equivalent). Make sure yours documents:
- Where `CHANGELOG.md`, `TODO.md`, and `SPEC.md` live
- Which version manifests to update (e.g. `package.json`, `pyproject.toml`, multiple workspace packages)
- The test command (`npm test`, `pytest`, `cargo test`, etc.)
- The build command and whether release artifacts should be attached to the GitHub Release
- Any docs-generation step (`npm run docs:generate`, `make docs`, etc.)
