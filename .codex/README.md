# Codex Assets

This directory contains Codex-specific assets for the Python agent coding
guidelines repository.

## Instruction File

`AGENTS.md` is the repo-local instruction file. Codex reads it when working in
this repository, so it remains the canonical local guidance.

## Repo-Local Skill

`skills/python-agent-coding-guidelines/` packages the guidance from
`.claude/CLAUDE.md` as a reusable Codex skill. The source instruction file is
optimized for Claude Code; the skill adapts the same Python coding-agent rules
for explicit Codex invocation.

Example prompts:

```text
Use $python-agent-coding-guidelines while refactoring this Python
module. Preserve behavior, keep the diff small, and run the relevant uv checks.
```

```text
Use $python-agent-coding-guidelines to review this change for naming,
function size, speculative abstractions, and uv workflow issues.
```

```text
Use $python-agent-coding-guidelines while adding this feature. If the
existing code is not open to the change, first propose the smallest safe
refactor.
```

## Discovery

This skill is versioned inside the repo at:

```text
.codex/skills/python-agent-coding-guidelines/
```

Depending on the Codex environment, repo-local skills may need to be installed or
synced into the user's Codex skills directory before they are available
globally.
