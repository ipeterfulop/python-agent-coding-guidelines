---
name: python-agent-coding-guidelines
description: Use when working on Python repositories where Claude should apply uv-first, reviewable coding guidelines from this repo's CLAUDE.md - preserve behavior, make small diffs, avoid speculative abstractions, improve touched code, use clear names and type hints, and run pytest, ruff, and type-check checks through uv. Trigger when editing, refactoring, or reviewing Python code; deciding between procedural and OOP designs; cleaning up AI-generated Python code; or making any dependency or environment change in a uv-managed project.
---

# Python Agent Coding Guidelines

Apply the Python coding-agent rules from this repository's `.claude/CLAUDE.md`
as a reusable workflow for Python code changes.

Use this skill when:

- working in a Python repository that should follow `uv`-first workflows
- improving AI-generated or hard-to-review Python code
- refactoring while preserving behavior
- deciding whether procedural code, OOP, or a small hybrid design fits a change
- reviewing or changing code where naming, function size, type hints, tests, or
  dependency hygiene matter

## Workflow

1. Read the target repository before editing. Identify the current project style,
   toolchain, tests, and whether the requested change is behavioral,
   refactoring-only, or both.
2. Preserve existing behavior unless the user explicitly asks to change it.
3. Choose the simplest maintainable design: procedural, OOP, or a small hybrid.
   Ask only when that choice materially changes the structure and the repo does
   not make the direction clear.
4. Keep diffs small and reviewable. Separate refactoring from behavior changes
   whenever practical.
5. Improve low-quality patterns only in code you already touch, unless a broader
   cleanup is explicitly requested.
6. Use `uv` for project commands and dependency changes.
7. Run the relevant tests/checks before finishing, or explain why they could not
   be run.

## References

Load only the reference needed for the task:

- [references/code-quality.md](references/code-quality.md): naming, function
  size, constants, Python idioms, and touched-code cleanup.
- [references/refactoring.md](references/refactoring.md): smell-driven
  refactoring, OOP decisions, extract-class guidance, and staged changes.
- [references/uv-workflow.md](references/uv-workflow.md): package operations,
  execution commands, project initialization, tests, ruff, type checking, and
  pre-commit.
- [references/safety-checklist.md](references/safety-checklist.md): hard
  constraints, package-safety guardrails, and final self-check.

## Default Stance

- Correctness, readability, reviewability, and maintainability beat cleverness.
- Shameless green code is acceptable until duplication has a clear shared reason
  to change.
- Do not introduce an abstraction on the first write.
- Names should describe exact behavior and sit at the right abstraction level.
- New or changed Python code should have useful type hints.
- Tests should assert behavior, not implementation details.
