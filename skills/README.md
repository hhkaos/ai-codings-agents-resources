
# Skills

Reusable AI coding agent skills — slash commands, prompt recipes, or instruction snippets depending on the agent.

Each skill lives in its own folder and includes:
- `prompt.md` — the canonical prompt content, agent-agnostic and without front matter
- `README.md` — what the skill does, when to use it, and any customisation notes

> **Terminology note:** "Skills" is what this repo calls reusable prompt recipes — a neutral term. Each agent has its own name for the same concept: slash commands (Claude Code), prompt files (Copilot), agent rules (Cursor), or inline sections (Codex/AGENTS.md).

---


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Skills](#skills)
  - [Available skills](#available-skills)
  - [How to install a skill](#how-to-install-a-skill)
    - [Claude Code](#claude-code)
    - [GitHub Copilot](#github-copilot)
    - [Cursor](#cursor)
    - [OpenAI Codex](#openai-codex)
    - [Gemini CLI](#gemini-cli)
  - [Quick install via agent chat](#quick-install-via-agent-chat)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Available skills

| Skill | Description |
|---|---|
| [`init-spec`](init-spec/) | Creates `SPEC.md` from scratch by interviewing you |
| [`review-spec`](review-spec/) | Interviews you about your existing spec and updates it |
| [`init-memory`](init-memory/) | Interactively generates AI context files for your project |
| [`ship`](ship/) | Stage changes, update CHANGELOG/TODO/SPEC, commit, and push |
| [`release`](release/) | Version bump, CHANGELOG update, git tag, and GitHub Release |

---

## How to install a skill

`prompt.md` is the agent-agnostic content. To install a skill, copy its `prompt.md` content into the right file for your agent and add the required front matter.

### Claude Code

File: `.claude/commands/<skill-name>.md`

```yaml
---
name: <skill-name>
description: <one-line description>
disable-model-invocation: true
---

[paste prompt.md content here]
```

Invoke with `/<skill-name>` in any Claude Code session. `disable-model-invocation: true` makes the skill run inline without spawning a sub-model.

---

### GitHub Copilot

File: `.github/prompts/<skill-name>.prompt.md`

```yaml
---
mode: agent
description: <one-line description>
---

[paste prompt.md content here]
```

Invoke with `/<skill-name>` in Copilot Chat (VS Code).

---

### Cursor

File: `.cursor/rules/<skill-name>.mdc`

```yaml
---
description: <one-line description>
globs:
alwaysApply: false
---

[paste prompt.md content here]
```

Trigger by saying the skill name or _"run /<skill-name>"_ in Cursor Chat.

---

### OpenAI Codex

Add as a named section in `AGENTS.md` (no front matter needed):

```markdown
## Prompt: <Skill name>

When asked to run "<skill-name>":

[paste prompt.md content here]
```

---

### Gemini CLI

Add as a named section in `GEMINI.md` or paste directly into chat (no front matter needed):

```markdown
## Prompt: <Skill name>

[paste prompt.md content here]
```

---

## Quick install via agent chat

Instead of creating files manually, paste this prompt into your agent's chat:

```
I want to add a new skill to this project called "<skill-name>".
Here is the skill prompt:

---
[paste prompt.md content here]
---

Please create the appropriate file for your agent type and add the correct front matter.
```

The agent will create the file in the right location for its own format.
