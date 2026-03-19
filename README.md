# AI Coding Agent Templates

A collection of reusable skills, templates, and guides for setting up AI coding agents on new or existing projects.

Agent-agnostic: works with Claude Code, GitHub Copilot, OpenAI Codex, Cursor, Gemini CLI, and others. Each agent calls things differently — see the [terminology guide](guides/agent-terminology.md) if you're coming from a specific tool.

## How to use this repo

Browse what you need and copy it into your project. No install, no config — just copy the files you want.

---

## What's in here

### [`skills/`](skills/)
Reusable slash commands / prompt recipes for AI coding agents. Each skill has installation instructions for each supported agent.

| Skill | What it does |
|---|---|
| [`init-spec`](skills/init-spec/) | Creates `SPEC.md` from scratch by interviewing you about the product |
| [`review-spec`](skills/review-spec/) | Interviews you about your existing `SPEC.md` and updates it |
| [`init-memory`](skills/init-memory/) | Interactively generates AI context files (CLAUDE.md, AGENTS.md, copilot-instructions.md, etc.) for your project |

### [`agents/`](agents/)
Per-agent setup guides and file templates.

| Agent | Guide |
|---|---|
| [Claude Code](agents/claude-code/) | Skills setup, CLAUDE.md format, front matter reference |
| [GitHub Copilot](agents/github-copilot/) | copilot-instructions.md setup, prompt files |
| [OpenAI Codex](agents/codex/) | AGENTS.md format and usage |

### [`guides/`](guides/)
Agent-agnostic workflow documentation — lessons learned, patterns, and checklists.

| Guide | What it covers |
|---|---|
| [Agent terminology](guides/agent-terminology.md) | How terms map across agents — skills, context files, front matter, invocation |
| [New project checklist](guides/new-project-checklist.md) | Step-by-step setup for a new project with AI agent support |
| [Workflow patterns](guides/workflow-patterns.md) | Reusable patterns: git conventions, monorepo setup, CI/CD, security, i18n |

### [`examples/`](examples/)
Real-world context file examples from actual projects.

| Example | Description |
|---|---|
| [ArcGIS mapping app](examples/arcgis-mapping/) | CLAUDE.md for a Vite + @arcgis/core v5 + Calcite project |

---

## Status and quality

Most of the content in this repo was written by an AI coding agent based on instructions and decisions made by the author. Not everything has been manually reviewed, so there may be errors, inconsistencies, or instructions that don't work as described.

If you find something wrong or confusing, please [open an issue](https://github.com/hhkaos/ai-codings-agents-resources/issues) — contributions and corrections are welcome.

---

## Contributing

Add a new skill by creating `skills/<skill-name>/prompt.md` and `skills/<skill-name>/README.md`. Follow the pattern of existing skills.

Add a new agent by creating `agents/<agent-name>/README.md` documenting: how to install skills, where config files go, and any front matter requirements.
