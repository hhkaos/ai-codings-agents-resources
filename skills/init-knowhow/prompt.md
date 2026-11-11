As a project grows, so does the experience of the humans and agents working
on it. That experience should not stay in old conversations: each new session
should start with it, so that work comes out right the first time and quality
improves instead of degrading.

This skill runs **once**. It does three things:

- inventories every piece of know-how in the repo,
- reorganizes it into a tiered, versioned structure,
- **installs a protocol** that makes agents consult and grow that know-how in
  every later session.

After setup the system evolves on its own. Rerun this skill only for a deep
audit.

---


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Principles (also copied into the installed protocol)](#principles-also-copied-into-the-installed-protocol)
- [Step 1 — Check the workspace before touching anything](#step-1--check-the-workspace-before-touching-anything)
- [Step 2 — Inventory every place know-how may live](#step-2--inventory-every-place-know-how-may-live)
- [Step 3 — Classify and select](#step-3--classify-and-select)
- [Step 4 — Propose the target structure](#step-4--propose-the-target-structure)
- [Step 5 — Present the plan; apply only after an explicit OK](#step-5--present-the-plan-apply-only-after-an-explicit-ok)
- [Protocol template — `docs/knowhow/README.md`](#protocol-template--docsknowhowreadmemd)
- [Finish](#finish)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Principles (also copied into the installed protocol)

1. **Context is a budget.** Every always-loaded line is paid for by every
   session. Keep only what is important. Dropping or not recording is a
   normal, good outcome.
2. **Guardrail before prose.** If a test, check script, helper, safer
   default, lint rule, or tool hook can make a mistake impossible or loud,
   prefer it. Prose shrinks to a pointer, or disappears.
3. **Scope precisely.** A preference or rejection applies only to what was
   said. Record what, where, and why; ask when the scope is ambiguous; never
   widen it.
4. **Propose, don't write.** Know-how changes need the user's explicit OK.
5. **Challenge with arguments, not compliance.** When a request looks wrong,
   ambiguous, too broad, contradicts recorded know-how or the code, or misses
   something:
   - say so once, briefly, with concrete evidence and a recommendation;
   - once the user decides, follow the decision and don't re-argue it.

## Step 1 — Check the workspace before touching anything

- Run `git status --short`, `git worktree list`, and look at recent commits.
- Guidance files may be edited concurrently by other agents or worktrees.
  List any uncommitted or worktree changes to the files you will reorganize
  and decide with the user how to sequence them. Never overwrite, revert, or
  stash someone else's changes.

## Step 2 — Inventory every place know-how may live

Search broadly; don't assume a layout.

- **Agent instruction files, at any depth:**
  - `AGENTS.md`, `CLAUDE.md`, `CLAUDE.local.md`, `GEMINI.md`
  - `.claude/` (rules, skills, settings, hooks), `.agents/`, `.codex/`
  - `.cursor/rules/`, `.cursorrules`, `.windsurfrules`, `.clinerules`
  - `.github/copilot-instructions.md`
  - Imported files (`@path` includes).
- **Human docs:**
  - `README`, `CONTRIBUTING`, style guides
  - `docs/`: plans, ADRs, knowledge bases, handoffs, audits, postmortems,
    notes.

  Respect files marked as private scratch notes ("ignore this document"):
  list them, but don't mine them.
- **Conventions already encoded as guardrails:** lint and format configs,
  pre-commit hooks, CI, type checks, test helpers, regression tests. Note
  which documented rules they already enforce.
- **Code comments that carry warnings:** `NOTE`, `WARNING`, `HACK`, `XXX`,
  `FIXME`, "don't …", "must …".
- **Agent memory outside the repo:** the user's per-project auto-memory and
  global agent files.
- **Workspace signals:** worktrees and branches from several tools, which may
  indicate concurrent agents.
- **History, only if the user wants it:** `git log` (fix / broke /
  regression / revert / workaround), handoffs, audits. Present at most 10
  candidates, most severe first.

## Step 3 — Classify and select

For each item, record:

- **Source:** file § section.
- **Kind:**

  | Kind | Example |
  | --- | --- |
  | rule / convention | "Escape dynamic content with `esc()`" |
  | pitfall | a silent failure mode + the check that catches it |
  | taste | UI/UX, copy tone, code style, tooling preferences |
  | reference pattern | "Compact lists are built like `_edition_row`" |
  | rejected approach | what, where, why — narrowly scoped |
  | working agreement | pushback, commit/handoff rhythm, when to suggest a new session, concurrent agents, how to report |
  | domain knowledge | investigated external sources and what they mean |
  | design decision | alternatives considered and why one was chosen |
  | command | how to run, test, deploy |
  | task state | status, blockers, next steps of one task |

- **Audience:** every agent / one specific tool / humans only.
- **Status:** current / stale (cites things that no longer exist) /
  duplicate / contradictory. Check every cited file, function, class, and
  control against the code while classifying: entries go stale quietly
  (a constant removed, a link turned into a dialog button), and moving a
  stale entry just relocates the error.
- **Guardrail:** already enforced by one? Could one replace it?
- **Worth its cost?** Keep an item only if it meets at least two of:
  - likely to recur,
  - severe (production or data, security, public surface, core workflow),
  - silent (tests don't catch it, or it went unnoticed for long),
  - an owner expectation a good new developer wouldn't guess.

  Drop it if it is derivable from the code, a one-off, task status, or
  numbers that go stale.

## Step 4 — Propose the target structure

Adapt to what already works. Don't move a healthy domain knowledge base,
design doc, or README section; link it from the index instead.

- **Shared agent guide** (`AGENTS.md`, or whatever file all agents read):
  - Project context, commands, workflow, short rules, working agreements.
  - A `## Know-how` section (keep it under ~20 lines) with:
    - the language of entries;
    - the trigger (Step 5);
    - an **index**: one line per area — "touching X → read
      `docs/knowhow/x.md`" — plus links to the healthy docs left in place.
- **Tool-specific files** (`CLAUDE.md`, etc.): only what that tool alone
  needs. Import or point to the shared guide instead of duplicating it.
- **Area files** (`docs/knowhow/<area>.md`), one per area, with frontmatter:

  ```yaml
  ---
  area: css
  paths: ["src/**/*.css", "src/**/pages.py"]
  ---
  ```

  Sections, each optional: `## Rules`, `## Pitfalls`, `## Taste`,
  `## Reference patterns`, `## Rejected`. Entries are short: rule first in
  the imperative, then the failure mode, the check that catches it, a
  concrete pointer, and one "how this bit us" sentence.
- **Path-scoped auto-loading** where the tool supports it:
  - Claude Code: `.claude/rules/*.md` with `paths:` frontmatter, e.g. a
    symlink to each area file. Verify the tool follows symlinks.
  - Cursor: `.mdc` globs.

  Other tools rely on the index.
- **Guardrails that fail on their first run are findings, not noise.** A
  new check often uncovers a live bug, or several instances of the pitfall
  it encodes. Report them. If fixing them is a separate task, encode the
  current state as a **ratchet**: list the known offenders, fail when a new
  one appears, and also fail when one is fixed until the list is lowered,
  so a fix can't regress.
- **A staleness check as a test**, run with the normal suite rather than a
  script someone must remember: it verifies that the paths, `module.attr`
  symbols, and private helpers cited in backticks in the know-how files
  still exist. It will also catch the agent's own new entries.
- **Deterministic hooks** for working agreements that can be automated.
  Example: a session-start hook that prints `git worktree list`,
  `git status --short`, and recent commits, so agents see concurrent work
  without asking every time.
- **Protocol file** (`docs/knowhow/README.md`, template below): read on
  demand, never always-loaded.
- **Stays where it belongs:** design decisions in the design doc, task state
  in handoffs, personal cross-project preferences in the user's global agent
  file (unversioned; confirm before writing).

**Language.** Ask once and record the answer in `## Know-how`:

- English travels best if the know-how will be shared publicly.
- The owner may prefer their own language; agents follow instructions
  equally well in either.
- If the user's instructions or conversation are not in English, confirm
  explicitly rather than assume.
- Avoid mixing languages within one tier.

## Step 5 — Present the plan; apply only after an explicit OK

Present:

- **The inventory** as a table: source → kind → destination →
  keep / move / merge / rewrite / drop (with the reason for each drop).
- **Always-loaded line counts** before and after. The after count should be
  smaller, or the plan should justify why not. Count the real files after
  drafting them; don't estimate. Estimates tend to come in well below what
  gets written once working agreements and the index are added.
- **Proposed guardrails and hooks**, each as its own optional item.
- **Concurrency notes** from Step 1.

When applying:

- **Lossless.** Every kept item has a destination; every drop is listed with
  its reason.
- **Fix every link, import, and in-code reference** to moved content (tests
  and comments that cite "the pitfall in CLAUDE.md"), then grep for dangling
  references.
- **One concern per commit** when the repo's workflow allows.

Install the **trigger** in the always-loaded guide, adapted to its language:

> **Know-how.** Before planning a task, read the index below and open only
> the area files the task touches. If the request conflicts with recorded
> know-how, say so before starting. When a correction, stated preference,
> rejection, non-obvious approval, silent or long-lived bug, or repeated
> mistake appears, propose a capture following `docs/knowhow/README.md`.

## Protocol template — `docs/knowhow/README.md`

Write this file in the chosen language, adapted to the repo:

```markdown
# Know-how protocol

How agents consult and grow this project's know-how. Read on demand; the
always-loaded guide only carries the trigger and the index.

## Principles
<the five principles above>

## Consult — before planning a task
1. Open only the area files the task touches, plus any working agreement
   that applies.
2. If the request conflicts with a recorded rule, taste, or rejection, or
   the know-how suggests a better approach, say so before starting.
3. Mention in one line which entries shaped the approach.
4. Session signals — suggest an action only when it brings a concrete
   benefit:
   - a new task unrelated to a long or already compacted session → a fresh
     session with a self-contained handoff;
   - other worktrees, or foreign uncommitted changes in files you are about
     to touch → propose isolation (a worktree, a branch, splitting files)
     before editing.

## Capture — during and at the end of work
- **Signals:** corrections; stated preferences (product, code, or how the
  agent should work); non-obvious approvals; rejections; silent or
  long-lived bugs; repetition; wasted turns; collaboration friction.
- **Filter:** at least two of recurring / severe / silent / an owner
  expectation a newcomer wouldn't guess, and none of derivable from code /
  one-off / task status / numbers that go stale. If the user asks to record
  something that fails the filter, say why.
- **Guardrail first:** propose a test, check, helper, or hook before prose.
- **Scope:** record what, where, and why; ask when ambiguous; never widen
  it. Before replacing an entry that seems contradictory, check whether the
  scopes actually overlap. If the owner confirms a lesson but is unsure how
  far it reaches, record the confirmed scope as such, mark the rest as
  uncertain, and tell agents to ask when a case doesn't clearly fit.
- **Destination:** an area file + index line; the shared guide only for
  short cross-cutting rules; a tool file only for tool-specific things; the
  design doc for decisions; handoffs for task state.
- **Propose, don't write.** Batch proposals at natural pauses (end of task,
  before commit), using this format:

  > **Know-how candidate:** <one sentence>
  > - **Kind / why:** …
  > - **Scope:** where it applies; where it does not
  > - **Guardrail option:** …
  > - **Destination:** file § section
  > - **Proposed text:** exact diff
  > - **Replaces or removes:** …

- **Edit, don't append.** Update the existing entry on the topic; replace
  contradicted entries; no dates, test counts, or timings.

## Harvest — only when the user asks
Mine `git log` (fix / broke / regression / revert / workaround), handoffs,
and audits for lessons never written down. Drop what is already recorded,
already enforced by a test, or a one-off. Present at most 10 candidates,
most severe first, in the capture format.

## Review — prune
Propose a review when the always-loaded guide passes ~300 lines, when an
area file passes ~150 lines, or when the user asks:
- Check that every file, function, class, and flag an entry cites still
  exists. Run the staleness test if the repo has one, and check by hand
  what it can't see (flags, described behaviour).
- Flag duplicates, contradictions, rejections scoped too broadly, entries a
  guardrail now covers, and `paths:` that no longer match real files.
- Report the proposed changes with before/after line counts. Apply nothing
  without an OK.
```

## Finish

Report:

- the files written, moved, and dropped;
- the always-loaded line counts before and after;
- the guardrails installed or left as proposals, and any bugs they
  uncovered;
- what can only be verified in a fresh session (session-start hooks firing,
  path-scoped files actually loading) — ask the user to confirm it next
  time;
- anything left open (for example, a history harvest not yet done).
