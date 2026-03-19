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

## Install per agent

### Claude Code (native slash command)

Copy `prompt.md` to your project:

```
.claude/commands/ship.md
```

Invoke with `/ship` in any Claude Code session.

**Front matter required:**
```yaml
---
name: ship
description: Stage changes, generate a commit message, commit using a git alias, and push
disable-model-invocation: true
---
```

---

### GitHub Copilot (prompt file)

```
.github/prompts/ship.prompt.md
```

**Front matter required:**
```yaml
---
mode: agent
description: Stage changes, update CHANGELOG/TODO/SPEC, commit, and push in one guided flow.
---
```

Invoke with `/ship` in Copilot Chat.

---

### Cursor

```
.cursor/rules/ship.mdc
```

**Front matter required:**
```yaml
---
description: How to stage, commit and push changes when asked to ship
globs:
alwaysApply: false
---
```

Trigger by saying _"ship these changes"_ or _"run /ship"_ in Cursor Chat.

---

### OpenAI Codex / AGENTS.md

Add as a named section in `AGENTS.md`:

```markdown
## Prompt: Ship changes

When asked to "ship" or commit and push:

[paste prompt.md content here, minus the front matter]
```

---

## Customising for your project

The skill checks CLAUDE.md for the doc file paths your project uses (`TODO.md` vs `docs/TODO.md`, etc.). Make sure your CLAUDE.md documents:
- Where `CHANGELOG.md` lives
- Where `TODO.md` and `SPEC.md` live
- Any per-module scoping conventions for CHANGELOG entries (e.g. `App1:`, `App2:`)
