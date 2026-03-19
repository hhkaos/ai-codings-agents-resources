# Skill: `ship`

Stages changes, updates project documentation (CHANGELOG, TODO, SPEC), generates a commit message, and pushes — in one guided flow.

**What it does:**
1. Reviews `git status` and `git diff`
2. Updates CHANGELOG.md, TODO.md, and SPEC.md as needed
3. Stages files by name (never `git add -A`)
4. Proposes a conventional commit message
5. Asks whether to commit as human (`git ch`) or AI (`git cai`)
6. Commits and pushes

**When to use it:** Any time you're ready to ship a chunk of work and want the docs kept in sync automatically.

> Requires `git ch` and `git cai` aliases to be configured. See the [new project checklist](../../guides/new-project-checklist.md#3-git-configuration) for setup.

---

## Install

→ See [How to install a skill](../README.md#how-to-install-a-skill) for per-agent file paths, front matter, and the quick-install prompt.

**Suggested description for front matter:**
> Stage changes, update CHANGELOG/TODO/SPEC, generate a commit message, and push in one guided flow.

### Customising for your project

The skill checks the project's context file (CLAUDE.md, AGENTS.md, or equivalent) for doc file paths. Make sure yours documents:
- Where `CHANGELOG.md`, `TODO.md`, and `SPEC.md` live
- Any per-module scoping conventions for CHANGELOG entries (e.g. `App1:`, `App2:`)
