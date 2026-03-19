# Agents

Per-agent setup guides, file templates, and the shared context pattern.

---

## The shared context pattern

Instead of maintaining N near-identical context files for N agents, use a single `PROJECT.md` as the source of truth. Each agent's file stays minimal — it just points to `PROJECT.md` and adds agent-specific behaviour rules on top.

```
project/
├── PROJECT.md                           ← shared context (stack, architecture, commands, conventions)
├── CLAUDE.md                            ← "read PROJECT.md" + Claude Code behaviour rules
├── AGENTS.md                            ← "read PROJECT.md" + Codex behaviour rules
└── .github/
    └── copilot-instructions.md          ← "read PROJECT.md" + Copilot behaviour rules
```

**`PROJECT.md`** contains everything that's true regardless of which agent is reading it:
- Project overview
- Commands (with ports)
- Stack
- Architecture and file map
- Coding style
- Constraints and anti-patterns
- Git conventions and security rules
- Known issues table

**Each agent file** contains only what's specific to that agent:
- How to behave (tone, verbosity, when to ask vs. act)
- Tool-specific rules (e.g. Claude's `AskUserQuestion`, Copilot's prompt mode)
- A pointer to `PROJECT.md`

### Minimal agent file example

```markdown
# CLAUDE.md

Read `PROJECT.md` for full project context (stack, architecture, commands, conventions, git rules).

---

## Behaviour rules (Claude Code specific)

### Ask before destructive actions
Confirm before deleting files, force-pushing, or dropping packages.

### Keep responses concise
Prefer showing changed code over explaining it. Use markdown link syntax for file references.

### Closing the loop
After fixing a bug or making an architectural decision, ask:
> "Should I update PROJECT.md or SPEC.md to reflect what we learned?"
```

### When to use this pattern

- Projects used by more than one AI agent
- Teams where different developers use different tools
- When you find yourself copy-pasting the same project info into multiple context files

### When a single agent file is fine

- Solo projects with one agent
- Simple projects where the context file stays small

---

## Templates

| Template | Description |
|---|---|
| [`PROJECT.md.template`](PROJECT.md.template) | Shared agent-agnostic project context |
| [`claude-code/CLAUDE.md.template`](claude-code/CLAUDE.md.template) | Full standalone CLAUDE.md (no shared PROJECT.md) |

---

## Agent guides

| Agent | Guide |
|---|---|
| [Claude Code](claude-code/) | Skills setup, CLAUDE.md format, front matter reference |
| [GitHub Copilot](github-copilot/) | copilot-instructions.md setup, prompt files |
| [OpenAI Codex](codex/) | AGENTS.md format and usage |
