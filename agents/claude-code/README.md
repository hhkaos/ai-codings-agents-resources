<!-- DOCTOC SKIP -->
# Claude Code Setup Guide

[Claude Code](https://claude.ai/code) is Anthropic's AI coding CLI. It reads project context from `CLAUDE.md` and supports custom slash commands via `.claude/commands/`.

---

## Project context file: `CLAUDE.md`

Claude Code reads `CLAUDE.md` at the start of every session. Place it at the root of your repo.

Use the [`CLAUDE.md.template`](CLAUDE.md.template) in this folder as a starting point, or run the [`/init-memory`](../../skills/init-memory/) skill to generate one interactively.

**Key sections to include:**
- Project overview and purpose
- Quick commands (install, dev with port, test, lint, build)
- Architecture and file map
- Coding style rules
- Behaviour rules (what Claude should always/never do)
- Git conventions and security rules
- Known issues / gotchas table

---

## Skills (custom slash commands)

Skills live in `.claude/commands/` in your project. Each skill is a markdown file with front matter.

**Directory structure:**
```
your-project/
└── .claude/
    └── commands/
        ├── init-memory.md
        └── review-spec.md
```

**Front matter fields:**

| Field | Required | Description |
|---|---|---|
| `name` | Yes | The slash command name (e.g. `init-memory` → `/init-memory`) |
| `description` | Yes | Shown in the `/` command picker |
| `disable-model-invocation` | No | Set `true` to run the skill inline without spawning a sub-model |

**Example:**
```yaml
---
name: review-spec
description: Review SPEC.md and update it based on an in-depth interview.
disable-model-invocation: true
---

[skill content here]
```

**To install a skill from this repo:** copy `skills/<skill-name>/prompt.md` into `.claude/commands/<skill-name>.md` in your project.

---

## Available skills

| Skill | Install path | Invoke with |
|---|---|---|
| [init-memory](../../skills/init-memory/) | `.claude/commands/init-memory.md` | `/init-memory` |
| [review-spec](../../skills/review-spec/) | `.claude/commands/review-spec.md` | `/review-spec` |

---

## Tips

- Claude Code also reads `CLAUDE.md` files in subdirectories — useful for monorepos where each app has its own context.
- Keep `CLAUDE.md` focused on **how to work** in the project, not what to build (that belongs in `SPEC.md`).
- Add a "Closing the loop" section to prompt Claude to update context files after bugs, discoveries, or milestones. See the template for the exact wording.
