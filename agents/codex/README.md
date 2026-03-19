# OpenAI Codex CLI Setup Guide

OpenAI Codex CLI reads project context from `AGENTS.md` at the root of your repo. It does not support slash commands — skills are embedded as named prompt sections and triggered manually.

---

## Project context file: `AGENTS.md`

Place your instructions at:

```
AGENTS.md
```

This file is read automatically by Codex at the start of each session. Plain markdown — no front matter.

**What to include:**
- Project overview
- Tech stack
- Coding conventions
- Commands (install, dev, test, build)
- File map / architecture
- Things the agent should always or never do
- Security and git rules

**Example opening:**
```markdown
Project-level instructions for the AI agent. Read this at the start of every session.

## Project

[2–3 sentences describing the project.]

## Commands

\`\`\`bash
npm install
npm run dev      # http://localhost:5173
npm test
npm run build
\`\`\`

## Coding style

- Plain JavaScript (ES modules). No TypeScript unless the project already uses it.
- No over-engineering — three similar lines > premature abstraction.

## Never do

- Never commit `.env` or private keys.
- Never use `git add -A` — stage by file name.
```

Use the `/init-memory` skill (see [skills/init-memory](../../skills/init-memory/)) to generate `AGENTS.md` alongside your other agent context files. Select "OpenAI Codex CLI" as a target agent during the interview.

---

## Skills (manual prompt recipes)

Codex has no slash command system. To reuse a skill, embed it as a named section in `AGENTS.md` and trigger it by asking the agent to run it by name.

**Pattern:**
```markdown
## Prompt: <skill name>

When asked to "<trigger phrase>", follow this process:

[skill content here — paste from prompt.md, minus the front matter]
```

**To install a skill from this repo:** paste the content of `skills/<skill-name>/prompt.md` (without front matter) into a `## Prompt: <name>` section in your `AGENTS.md`.

---

## Available skills

| Skill | Trigger phrase | Source |
|---|---|---|
| [init-memory](../../skills/init-memory/) | "Generate context files" or "Run init-memory" | Paste into `AGENTS.md` |
| [review-spec](../../skills/review-spec/) | "Review the spec" or "Run review-spec" | Paste into `AGENTS.md` |

---

## Tips

- `AGENTS.md` and `CLAUDE.md` can coexist in the same repo — each agent reads only its own file.
- Keep `AGENTS.md` focused on rules and context, not inline documentation. Link to other files rather than duplicating them.
- Codex works best when behaviour rules are short, imperative, and specific — avoid vague instructions like "be helpful".
