
Follow these steps to commit and push changes:


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. Review changes](#1-review-changes)
- [2. Update project documentation](#2-update-project-documentation)
  - [CHANGELOG.md](#changelogmd)
  - [TODO.md](#todomd)
  - [SPEC.md](#specmd)
- [3. Stage files](#3-stage-files)
- [4. Generate a commit message](#4-generate-a-commit-message)
- [5. Choose the git alias](#5-choose-the-git-alias)
- [6. Commit](#6-commit)
- [7. Push](#7-push)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 1. Review changes

Run `git status` (never use `-uall`) and `git diff` (both staged and unstaged) to understand all current changes.

## 2. Update project documentation

Review the changes and update the following files as needed. Check CLAUDE.md (or equivalent context file) to confirm the exact paths used in this project.

### CHANGELOG.md
Read `CHANGELOG.md` and add an entry under the `[Unreleased]` section describing the change. Place it under the appropriate subsection (`Added`, `Changed`, `Fixed`, `Removed`). Create the subsection if it doesn't exist. Keep entries concise — one bullet point per logical change. If the project has multiple apps or modules, prefix each entry with the relevant scope (e.g. `App1:`, `App2:`, `Docs:`) so readers know the impact at a glance.

### TODO.md
Read `TODO.md` (or `docs/TODO.md` — check CLAUDE.md for the project's convention) and update checkboxes or status labels to reflect the current state. For example:
- Mark completed tasks as `[x]`
- Update phase status labels (e.g. `⬜ TODO` → `🔧 IN PROGRESS` → `✅ COMPLETE`)
- Only modify items directly related to the current changes

### SPEC.md
Read `SPEC.md` (or `docs/SPEC.md`) and update it only if the changes affect the technical specification:
- New or changed features that alter documented architecture or behaviour
- New acceptance criteria to check off
- Updated technology choices or dependencies
- Skip this file if the changes are purely internal (refactoring, tests, tooling)

## 3. Stage files

Stage the relevant files by name, including any updated documentation files. Never use `git add -A` or `git add .`. Never stage files that may contain secrets (`.env`, credentials, private keys, etc.) — warn the user if any are detected.

## 4. Generate a commit message

- Run `git log --oneline -10` to see recent commit style.
- Analyze the staged diff and draft a concise conventional commit message (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`).
- Focus on the "why" rather than the "what".
- Keep it to 1–2 sentences.
- Show the proposed message to the user before committing.

## 5. Choose the git alias

Ask the user which alias to use:

- **`git cai`** — AI-attributed commit (sets a distinct author and prefixes the message with `AI: `)
- **`git ch`** — Regular commit with the user's default git identity

## 6. Commit

Run the chosen alias with the commit message. For example:
- `git cai "feat: add dark mode toggle to settings page"`
- `git ch "feat: add dark mode toggle to settings page"`

## 7. Push

Run `git push`. If the branch has no upstream, use `git push -u origin <branch>`.
