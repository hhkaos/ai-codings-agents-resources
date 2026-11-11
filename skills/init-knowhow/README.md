
# Skill: `init-knowhow`

One-time setup that turns a repository's scattered agent guidance into a self-maintaining know-how system. Every later agent session then starts with the project's accumulated judgment: expectations, recurring mistakes, latent gotchas, the owner's taste, rejected approaches, and how the owner expects agents to collaborate. Always-loaded context stays small.

**What it does:**
1. Checks the workspace for concurrent work (uncommitted changes, other worktrees) before touching anything
2. Inventories every place know-how may live: agent files at any depth, docs, lint/CI/test guardrails, warning comments, agent memory, and optionally git history
3. Classifies each item (kind, audience, status, guardrail candidate, worth its token cost) and drops what doesn't earn its place
4. Proposes a tiered structure: a short shared guide with a `## Know-how` index, per-area files loaded on demand, tool-specific files that only point to the shared guide, and deterministic guardrails and hooks
5. Applies the plan only after an explicit OK, losslessly, fixing every link
6. Installs a protocol (consult, capture, harvest, prune) so the know-how keeps growing without rerunning the skill

**When to use it:** On an existing project whose context files have grown long, drifted, or are split across tools. Run it once; rerun it only for a deep audit. For a new project with no guidance yet, start with [`init-memory`](../init-memory/).

---


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Install](#install)
  - [Agent-specific notes](#agent-specific-notes)
- [Notes from the pilot](#notes-from-the-pilot)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Install

→ See [How to install a skill](../README.md#how-to-install-a-skill) for per-agent file paths, front matter, and the quick-install prompt.

**Suggested description for front matter:**
> One-time setup (rerunnable as an audit) that turns a repo's scattered agent guidance into a self-maintaining know-how system without bloating always-loaded context.

### Agent-specific notes

- **Claude Code** — area files can load automatically when matching files are touched: put them (or symlinks to them) in `.claude/rules/` with `paths:` frontmatter. A `SessionStart` hook in `.claude/settings.json` can print `git worktree list`, `git status --short`, and recent commits so concurrent work is visible from the first turn. Both only take effect in a new session, so confirm them there.
- **OpenAI Codex** — reads `AGENTS.md`, so the shared guide and its `## Know-how` index carry the whole system. There is no path-scoped loading; agents open area files from the index.
- **Cursor** — area files can become `.cursor/rules/*.mdc` with `globs:` matching their `paths:`.
- **GitHub Copilot** — no path-scoped loading for these files; point `.github/copilot-instructions.md` at the shared guide.

## Notes from the pilot

The skill was piloted on a mid-sized web app worked on by two agent tools at once. Lessons from that run are already folded into the prompt:

- **Entries go stale quietly.** Moving content surfaced references to code that no longer existed. Verify while classifying, then keep a staleness test in the suite.
- **New guardrails find real bugs.** One check turned up a "Clear all" button that left a filter set. A query-count check turned up per-row queries on most public pages, now kept as a ratchet until they are fixed.
- **Measure line counts; don't estimate them.** The always-loaded estimate was well below the real result.
- **Record uncertain scope as uncertain.** When the owner confirms a lesson but isn't sure how far it reaches, the entry says so and tells agents to ask.
