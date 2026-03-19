
# Agent terminology guide

Each AI coding agent uses different names for the same concepts. This guide maps the terms used in this repo to the equivalent in each agent, so you can apply any skill or template regardless of which tool you use.

---


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Term mapping](#term-mapping)
- [Concepts explained](#concepts-explained)
  - [Skill](#skill)
  - [Context file](#context-file)
  - [Front matter](#front-matter)
- [Convention used in this repo](#convention-used-in-this-repo)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Term mapping

| This repo | Concept | Claude Code | GitHub Copilot | Cursor | OpenAI Codex | Gemini CLI |
|---|---|---|---|---|---|---|
| **skill** | A reusable prompt recipe that can be invoked on demand | slash command | prompt file | agent rule | prompt section in AGENTS.md | prompt section in GEMINI.md |
| **context file** | The main markdown file an agent reads at the start of every session | `CLAUDE.md` | `.github/copilot-instructions.md` | `.cursorrules` / `.cursor/rules/` | `AGENTS.md` | `GEMINI.md` |
| **prompt.md** | The canonical, agent-agnostic source for a skill's content | copied to `.claude/commands/<name>.md` | content pasted into `.github/prompts/<name>.prompt.md` | content pasted into `.cursor/rules/<name>.mdc` | content pasted into `AGENTS.md` section | content pasted into `GEMINI.md` section |
| **front matter** | YAML metadata at the top of a skill file that configures how the agent handles it | required (`name`, `description`, optional `disable-model-invocation`) | required (`mode`, `description`) | required (`description`, `globs`, `alwaysApply`) | not used | not used |
| **invocation** | How you trigger a skill during a session | `/skill-name` in chat | `/skill-name` in Copilot Chat | say _"run skill-name"_ in chat | say _"run skill-name"_ in chat | say _"run skill-name"_ in chat |

---

## Concepts explained

### Skill

A skill is a reusable prompt recipe: a set of instructions that tells the agent how to perform a specific task (e.g. "interview me and create a SPEC.md"). In this repo, every skill lives in `skills/<name>/` and has two files:

- `prompt.md` — the agent-agnostic content
- `README.md` — install instructions for each supported agent

Each agent installs skills differently:

- **Claude Code** — copy `prompt.md` to `.claude/commands/<name>.md`. Native slash command support: `/name` appears in the command picker.
- **GitHub Copilot** — paste the content into `.github/prompts/<name>.prompt.md` with Copilot-specific front matter. Invoked with `/name` in Copilot Chat.
- **Cursor** — paste the content into `.cursor/rules/<name>.mdc`. Cursor rules are always-on or description-triggered, not slash commands — trigger by asking the agent in chat.
- **OpenAI Codex** — paste the content (without front matter) as a named `## Prompt:` section in `AGENTS.md`. Trigger by asking the agent to run it by name.
- **Gemini CLI** — same as Codex: paste into `GEMINI.md` or send directly in chat.

### Context file

The context file is the markdown file an agent reads automatically at the start of every session. It defines who the agent is in this project: stack, conventions, behaviour rules, git policies, etc.

In this repo, `PROJECT.md` is the **shared, agent-agnostic** source of truth. Each agent's context file is thin — it references `PROJECT.md` and only adds agent-specific behaviour on top. This avoids duplicating project info across multiple files.

### Front matter

Front matter is a YAML block at the top of a skill file (between `---` delimiters) that tells the agent how to handle the file. Not all agents use it:

- **Claude Code** uses it to set the slash command name, description, and optional flags.
- **GitHub Copilot** uses it to set the mode (`ask`, `edit`, or `agent`) and description.
- **Cursor** uses it to set a trigger description, glob scope, and whether the rule is always-on.
- **Codex and Gemini CLI** do not use front matter — the content is pasted as plain markdown.

When you install a skill for Codex or Gemini CLI, strip the front matter block and paste only the body.

---

## Convention used in this repo

This repo uses **"skill"** as the neutral term for a reusable prompt recipe, regardless of agent. The folder is `skills/`, the canonical file is `prompt.md`. No agent-specific term was chosen to avoid implying any agent is the default.

If a term in a README feels unfamiliar, check the mapping table above.
