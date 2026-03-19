# Skills

Reusable AI coding agent skills — slash commands, prompt recipes, or instruction snippets depending on the agent.

Each skill lives in its own folder and includes:
- `prompt.md` — the canonical prompt content (agent-agnostic)
- `README.md` — what the skill does and how to install it for each supported agent

> **Terminology note:** "Skills" is what this repo calls reusable prompt recipes — a neutral term. Each agent has its own name for the same concept: slash commands (Claude Code), prompt files (Copilot), agent rules (Cursor), or inline sections (Codex/AGENTS.md).

---

## Available skills

| Skill | Description |
|---|---|
| [`init-spec`](init-spec/) | Creates `SPEC.md` from scratch by interviewing you |
| [`review-spec`](review-spec/) | Interviews you about your existing spec and updates it |
| [`init-memory`](init-memory/) | Interactively generates AI context files for your project |
| [`ship`](ship/) | Stage changes, update CHANGELOG/TODO/SPEC, commit, and push |
| `release` | _(coming soon)_ Version bump, git tag, and GitHub Release |

---

## How skills work per agent

Skills are natively supported as slash commands in **Claude Code**. Other agents have varying levels of support:

| Agent | Skill support | Mechanism |
|---|---|---|
| Claude Code | Native slash commands | `.claude/commands/<skill>.md` |
| GitHub Copilot | Prompt files (slash commands in Chat) | `.github/prompts/<skill>.prompt.md` |
| Cursor | Agent rules (always-on, not triggered) | `.cursor/rules/<skill>.mdc` |
| OpenAI Codex | Manual — paste or reference in AGENTS.md | `AGENTS.md` section |
| Gemini CLI | Manual — paste into GEMINI.md or chat | `GEMINI.md` section |

See each skill's `README.md` for exact file contents and front matter per agent.
