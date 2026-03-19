# ai-templates

> A public collection of reusable skills, templates, and guides for setting up AI coding agents on new or existing projects.

This file is the shared AI context for this project. It is read by all AI coding agents.
Agent-specific behaviour rules live in each agent's own file (CLAUDE.md, AGENTS.md, etc.).

---

## What this project is

A brain dump / knowledge base for developers who use AI coding agents. The goal is to make it easy to reuse skills (slash commands), context file templates, and workflow patterns across projects — regardless of which AI agent they use.

It is a **documentation-only repo** — no build step, no install, no tests. All files are markdown.

---

## Structure

```
ai-templates/
├── PROJECT.md                          # Shared AI context (this file)
├── SPEC.md                             # Product specification — source of truth for requirements
├── CLAUDE.md                           # Claude Code behaviour rules (references PROJECT.md)
├── AGENTS.md                           # OpenAI Codex behaviour rules (references PROJECT.md)
├── .github/
│   └── copilot-instructions.md         # GitHub Copilot behaviour rules (references PROJECT.md)
├── README.md                           # Entry point for humans
├── skills/                             # Reusable slash commands / prompt recipes
│   ├── README.md                       # Agent support matrix
│   ├── init-spec/                      # Creates SPEC.md from scratch via interview
│   │   ├── prompt.md                    # Canonical skill content (agent-agnostic)
│   │   └── README.md                   # Per-agent install guide
│   ├── review-spec/                    # Deepens and updates an existing SPEC.md
│   │   ├── prompt.md
│   │   └── README.md
│   ├── init-memory/                    # Generates PROJECT.md + agent context files
│   │   ├── prompt.md
│   │   └── README.md
│   └── ship/                           # Commit + push with doc updates
│       ├── prompt.md
│       └── README.md
├── agents/                             # Per-agent setup guides and templates
│   ├── README.md                       # Shared PROJECT.md pattern explained
│   ├── PROJECT.md.template             # Template for shared context files
│   ├── claude-code/
│   │   ├── README.md                   # Skills setup, front matter reference
│   │   └── CLAUDE.md.template          # Full standalone CLAUDE.md template
│   ├── github-copilot/
│   │   └── README.md
│   └── codex/
│       └── README.md
├── guides/
│   └── new-project-checklist.md        # Step-by-step new project setup
└── examples/
    └── arcgis-maps-sdk-js/
        ├── README.md
        └── vite-calcite/
            └── CLAUDE.md.example       # Real CLAUDE.md from an ArcGIS + Vite + Calcite project
```

---

## Key concepts

### Skills
Each skill lives in `skills/<name>/` and has two files:
- `prompt.md` — the canonical content in Claude Code front matter format
- `README.md` — per-agent install table (file location, front matter, invocation)

Skills are native slash commands in Claude Code (`.claude/commands/`), prompt files in Copilot (`.github/prompts/`), rules in Cursor, and manual prompt sections in Codex/AGENTS.md.

### Shared context pattern
`PROJECT.md` (this file) is the agent-agnostic source of truth. Each agent's file references it and adds only agent-specific behaviour rules. This avoids duplicating project info across N context files.

### Examples
Organized by technology category (`arcgis-maps-sdk-js/`, future: `calcite-design-system/`, `native/`). Each example is a real context file from an actual project, scrubbed of private details.

---

## Contributing

- **New skill:** add `skills/<name>/prompt.md` + `skills/<name>/README.md`, update `skills/README.md` table and root `README.md`
- **New agent:** add `agents/<name>/README.md`, update `agents/README.md` and root `README.md`
- **New example:** add under `examples/<category>/<project-type>/`, following existing structure

---

## What this repo does NOT do

- No npm install, no build, no CI
- No agent-specific tooling in the repo itself — just documentation
- Examples are illustrative, not runnable projects

---

## Git conventions

- Conventional commits: `feat:`, `fix:`, `docs:`, `chore:`
- Stage files by name — never `git add -A`
- Never commit `MEMORY.md` or `memory/` (gitignored — Claude workspace memory, private)

---

## Behaviour rules (apply to all agents)

### This is a docs-only repo
No build, no install, no tests. All changes are markdown files. Don't suggest adding tooling unless explicitly asked.

### Keep the shared context pattern consistent
Every skill must have both `prompt.md` (canonical content) and `README.md` (per-agent install guide). Every new agent needs a `README.md` in `agents/<name>/`. When adding either, update the relevant index files (`skills/README.md`, `agents/README.md`, root `README.md`).

### Ask before destructive actions
Confirm before deleting files or folders — the repo structure is intentional.

### Keep responses concise
Prefer direct edits over long explanations.

---

## Closing the loop (always follow these)

### After adding a skill or agent
Ask: "Should I update `PROJECT.md` to reflect the new structure?"

### After any session that changes the repo shape
Ask: "Want me to update `PROJECT.md` or the root `README.md` with what changed?"

### When something in a context file turns out to be wrong
Correct it immediately — don't work around inaccurate instructions.
