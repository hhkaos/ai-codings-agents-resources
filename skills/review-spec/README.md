# Skill: `review-spec`

Reads your `SPEC.md` and interviews you about it — technical implementation, UI/UX decisions, tradeoffs, and improvements. Updates the spec with your answers.

**What it does:**
1. Reads `SPEC.md` in full
2. Asks in-depth, non-obvious questions — challenges assumptions and surfaces gaps
3. Updates `SPEC.md` with everything you discuss

**When to use it:** Before starting implementation, or any time the spec feels stale or underspecified.

> Requires a `SPEC.md` in the project root. If you don't have one yet, run `/init-spec` first.

---

## Install per agent

### Claude Code (native slash command)

Copy `prompt.md` to your project:

```
.claude/commands/review-spec.md
```

The file already has the correct front matter. Invoke with `/review-spec` in any Claude Code session.

**Front matter required:**
```yaml
---
name: review-spec
description: Review a project specification in detail, asking in-depth questions about technical implementation, UI/UX, concerns, tradeoffs, and potential improvements. Update the specification with the answers provided.
disable-model-invocation: true
---
```

> `disable-model-invocation: true` prevents Claude from auto-invoking a sub-model — the skill runs inline.

---

### GitHub Copilot (prompt file)

Copy `prompt.md` content to:

```
.github/prompts/review-spec.prompt.md
```

**Front matter required:**
```yaml
---
mode: agent
description: Review SPEC.md and interview the developer about technical decisions, tradeoffs, and improvements. Update the spec with answers.
---
```

Invoke with `/review-spec` in Copilot Chat (VS Code).

---

### Cursor (agent rule)

```
.cursor/rules/review-spec.mdc
```

**Front matter required:**
```yaml
---
description: How to review and update SPEC.md when asked
globs:
alwaysApply: false
---
```

Trigger by saying: _"Review my spec"_ or _"Run review-spec"_ in Cursor Chat.

---

### OpenAI Codex / AGENTS.md

Add as a named section in `AGENTS.md`:

```markdown
## Prompt: Review specification

When asked to review the spec or run "review-spec":

[paste prompt.md content here, minus the front matter]
```

---

### Gemini CLI

Embed in `GEMINI.md` or paste directly into chat:

```markdown
## Prompt: Review specification

[paste prompt.md content here, minus the front matter]
```
