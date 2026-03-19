# Skill: `init-spec`

Interviews you to create a `SPEC.md` from scratch — product purpose, users, core features, scope, constraints, and open questions.

**What it does:**
1. Checks if `SPEC.md` already exists (warns if so)
2. Asks 9 focused questions, one at a time
3. Writes `SPEC.md` with everything you discussed

**When to use it:** At the start of a new project, before running `init-memory`.

> After running this, use `/review-spec` to go deeper, or `/init-memory` to generate AI context files (it reads `SPEC.md` automatically).

---

## Install per agent

### Claude Code (native slash command)

Copy `prompt.md` to your project:

```
.claude/commands/init-spec.md
```

The file already has the correct front matter. Invoke with `/init-spec` in any Claude Code session.

**Front matter required:**
```yaml
---
name: init-spec
description: Interview the user to create a SPEC.md from scratch — product purpose, users, features, scope, constraints, and open questions.
disable-model-invocation: true
---
```

---

### GitHub Copilot (prompt file)

Copy `prompt.md` content to:

```
.github/prompts/init-spec.prompt.md
```

**Front matter required:**
```yaml
---
mode: agent
description: Interview the developer to create SPEC.md from scratch.
---
```

Invoke with `/init-spec` in Copilot Chat (VS Code).

---

### Cursor (agent rule)

```
.cursor/rules/init-spec.mdc
```

**Front matter required:**
```yaml
---
description: How to create SPEC.md from scratch when asked
globs:
alwaysApply: false
---
```

Trigger by saying: _"Create a spec"_ or _"Run init-spec"_ in Cursor Chat.

---

### OpenAI Codex / AGENTS.md

Add as a named section in `AGENTS.md`:

```markdown
## Prompt: Create specification

When asked to create a spec or run "init-spec":

[paste prompt.md content here, minus the front matter]
```

---

### Gemini CLI

Embed in `GEMINI.md` or paste directly into chat:

```markdown
## Prompt: Create specification

[paste prompt.md content here, minus the front matter]
```
