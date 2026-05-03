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

Then let the agent work normally. The point is not to micromanage every edit.
The point is to set a high enough default bar that generated code starts closer
to something a data engineer would actually want to maintain.
