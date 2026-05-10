# Python OOP Code Generation Guideline

This repo contains instruction files for AI coding agents that work on Python
projects. The goal is simple: make generated code easier to review, easier to
maintain, and less likely to turn into a pile of vague helper functions.

It is written from one engineer to another. If an agent is going to touch a
Python codebase, it should behave like a careful teammate: read first, make
small changes, preserve behavior, use the project toolchain, and leave the code
clearer than it found it.

## What is in here

- `.codex/AGENTS.md` is the Codex instruction file.
- `.claude/CLAUDE.md` is the Claude-specific version of the same guidance.
- `.codex/skills/python-agent-coding-guidelines/` is a repo-local
  Codex skill that packages the guidance for reusable, explicit invocation.
- `.claude/skills/python-agent-coding-guidelines/` is the equivalent
  repo-local Claude skill, with the same workflow and reference files in
  Claude's `SKILL.md` + `references/` layout.

The files are intentionally detailed because they are meant to shape day-to-day
coding behavior, not just describe preferences.

## What these instructions optimize for

They push agents toward:

- preserving existing behavior unless a change is explicitly requested
- small, reviewable diffs
- `uv`-first Python workflows
- clear names at the right level of abstraction
- type hints on new code
- tests and checks before handing work back
- simple code before speculative abstraction
- OOP only when it improves cohesion, clarity, or testability
- idiomatic use of comprehensions, generators, and context managers — with explicit defaults and decision checklists for when to reach for them and when not to

The guidance is especially useful for repos that contain AI-generated Python
code that is technically working but hard to read: long functions, procedural
scripts, repeated string literals, vague names, and logic that is hard to
review.

## The engineering stance

The core idea is not "make everything object-oriented." It is: choose the shape
that makes the code honest.

Sometimes that is a small function. Sometimes it is a frozen dataclass. Sometimes
it is a real domain object because the same primitive value is carrying too much
meaning across the codebase.

The instructions tell agents to avoid cleverness, avoid premature abstraction,
and name the smell they are removing when they refactor. That makes the work
easier to review and keeps the conversation grounded in concrete tradeoffs.

## Tooling expectations

The project standard is `uv`.

Agents should run project code and tools through `uv run`, modify dependencies
through `uv add` or `uv remove`, and avoid direct `pip`, manually managed virtual
environments, or hand-edited dependency lists.

That constraint matters. A lot of debugging time is wasted when an agent runs a
different Python environment than the one the project actually uses.

## How to use this

Copy the relevant instruction file into a Python repo, or keep this repo as the
source of truth and sync updates into agent-specific locations:

- Codex: `.codex/AGENTS.md`
- Claude: `.claude/CLAUDE.md`

For skill-based use, reference or install the repo-local skill at:

- Codex: `.codex/skills/python-agent-coding-guidelines/`
- Claude: `.claude/skills/python-agent-coding-guidelines/`

See `.claude/README.md` and `.codex/README.md` for example invocations and
discovery notes for each agent.

Then let the agent work normally. The point is not to micromanage every edit.
The point is to set a high enough default bar that generated code starts closer
to something a data engineer would actually want to maintain.

## IMPORTANT — global install vs. skill-only

The instruction file (`AGENTS.md` / `CLAUDE.md`) is **large on purpose**: ~1000
lines of detailed guidance. That is fine as a project-local file — agents only
load it when they are inside this repo. It is **not fine** as an unscoped
global instruction file.

If you drop the raw instructions into `~/.codex/AGENTS.md` or
`~/.claude/CLAUDE.md` without scoping, every conversation — TypeScript work,
infra edits, plain shell sessions, prose drafts — pays the cost:

- ~1000 lines of Python rules consume context that other tasks need.
- Rules like "use `uv run`" or "no `# type: ignore`" leak into non-Python
  contexts where they make no sense.
- Long global preambles can crowd out the project's own `CLAUDE.md` /
  `AGENTS.md`, since the model reads more before it ever sees the local file.

You have two safe options. Pick one:

1. **Scope it in the global instruction file.** Append the guidance under an
   explicit heading such as *"Python projects — apply only when working in a
   Python repository,"* so the model can ignore it when the repo is not
   Python. The global file in this user's setup uses this pattern; see
   `~/.claude/CLAUDE.md` for the structure.
2. **Rely solely on the skill.** Install only
   `~/.claude/skills/python-agent-coding-guidelines/` (or the Codex
   equivalent) and skip the global instruction file entirely. The skill's
   `description` is what triggers loading, so the bulk of the guidance is
   only pulled into context when the agent is actually doing Python work.
   This keeps the global footprint near zero and is the cleanest default.

Do **not** paste the unscoped instruction file into a global location. The
project-local files in this repo are already the right surface for that
content; the global surface should either scope it explicitly or leave it to
the skill.
