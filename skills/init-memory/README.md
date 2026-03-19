# Skill: `init-memory`

Interactively gathers project information and generates AI context files tailored to the agent(s) your team uses.

**What it does:**
1. Checks for an existing `SPEC.md` to pre-fill answers
2. Asks which AI agents your team uses
3. Runs a 15-question interview about your project (stack, architecture, git conventions, security rules, etc.)
4. Writes the correct context file(s) for each agent (`CLAUDE.md`, `AGENTS.md`, `copilot-instructions.md`, etc.)

**When to use it:** At the start of a new project, or when onboarding an AI agent to an existing one.

---

## Install per agent

### Claude Code (native slash command)

Copy `prompt.md` to your project:

```
.claude/commands/init-memory.md
```

The file already has the correct front matter. Invoke with `/init-memory` in any Claude Code session.

**Front matter required:**
```yaml
---
name: init-memory
description: Interactively gather project information and generate AI context files (CLAUDE.md, AGENTS.md, copilot-instructions.md, etc.)
---
```

---

### GitHub Copilot (prompt file)

Copy `prompt.md` content to:

```
.github/prompts/init-memory.prompt.md
```

**Front matter required:**
```yaml
---
mode: agent
description: Interactively gather project information and generate AI context files for the agents your team uses.
---
```

Invoke with `/init-memory` in Copilot Chat (VS Code). Requires GitHub Copilot Chat with prompt files enabled.

> **Note:** Copilot does not have the `AskUserQuestion` tool, so the skill will ask questions in plain text one at a time. The interaction instructions block in the skill handles this.

---

### Cursor (agent rule)

Cursor does not support triggered slash commands. Instead, add the skill content as an always-on agent rule:

```
.cursor/rules/init-memory.mdc
```

**Front matter required:**
```yaml
---
description: How to generate AI context files for this project when asked
globs:
alwaysApply: false
---
```

To trigger it, open Cursor Chat and say: _"Run the init-memory skill"_ or _"Generate context files for this project"_.

---

### OpenAI Codex / AGENTS.md

Codex does not support slash commands. Add the skill as a named section in your `AGENTS.md`:

```markdown
## Prompt: Generate AI context files

When asked to generate context files or run "init-memory", follow this process:

[paste prompt.md content here, minus the front matter]
```

Trigger by saying: _"Run the init-memory prompt"_ in your Codex session.

---

### Gemini CLI

Same approach as Codex — embed in `GEMINI.md` or paste directly into chat:

```markdown
## Prompt: Generate AI context files

[paste prompt.md content here, minus the front matter]
```
