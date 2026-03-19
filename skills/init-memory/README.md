# Skill: `init-memory`

Interactively gathers project information and generates AI context files tailored to the agent(s) your team uses.

**What it does:**
1. Checks for an existing `SPEC.md` to pre-fill answers
2. Asks which AI agents your team uses
3. Runs a 15-question interview about your project (stack, architecture, git conventions, security rules, etc.)
4. Writes the correct context file(s) for each agent (`CLAUDE.md`, `AGENTS.md`, `copilot-instructions.md`, etc.)

**When to use it:** At the start of a new project, or when onboarding an AI agent to an existing one.

---

## Install

→ See [How to install a skill](../README.md#how-to-install-a-skill) for per-agent file paths, front matter, and the quick-install prompt.

**Suggested description for front matter:**
> Interactively gather project information and generate AI context files for the agents your team uses.

### Agent-specific notes

- **Claude Code** — the `AskUserQuestion` tool is available; the skill uses it to ask questions interactively.
- **GitHub Copilot** — no `AskUserQuestion` tool; the skill asks questions in plain text one at a time.
- **Cursor** — does not support triggered slash commands; add as an `alwaysApply: false` rule and trigger by saying _"run the init-memory skill"_.
