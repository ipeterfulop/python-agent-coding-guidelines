# Claude Assets

This directory contains Claude-specific assets for the Python agent coding
guidelines repository.

## Instruction File

`CLAUDE.md` is the repo-local instruction file. Claude Code reads it when
working in this repository, so it remains the canonical local guidance.

## Repo-Local Skill

`skills/python-agent-coding-guidelines/` packages the same guidance as a
reusable Claude skill. The skill is useful when you want to apply these Python
coding-agent rules in another Python repository or invoke the guidance
explicitly during a task.

The skill follows Claude's standard layout:

```text
.claude/skills/python-agent-coding-guidelines/
├── SKILL.md                       # workflow and trigger description
└── references/
    ├── code-quality.md            # naming, function size, idioms
    ├── refactoring.md             # smells, OOP, extract-class
    ├── uv-workflow.md             # uv, pytest, ruff, type checking
    └── safety-checklist.md        # hard constraints and self-check
```

`SKILL.md` carries the YAML frontmatter (`name`, `description`) Claude uses to
decide when to load the skill. The references are loaded on demand from inside
the skill.

## Example Usages

The skill triggers automatically on Python work that matches its description.
You can also invoke it explicitly:

```text
Use the python-agent-coding-guidelines skill while refactoring this Python
module. Preserve behavior, keep the diff small, and run the relevant uv checks.
```

```text
Apply python-agent-coding-guidelines to review this change for naming,
function size, speculative abstractions, and uv workflow issues.
```

```text
Use python-agent-coding-guidelines while adding this feature. If the existing
code is not open to the change, first propose the smallest safe refactor.
```

```text
Following python-agent-coding-guidelines, scaffold a new uv project with
exclude-newer set to 14 days and a pytest + ruff + ty toolchain.
```

## Discovery

The skill is versioned inside the repo at:

```text
.claude/skills/python-agent-coding-guidelines/
```

Claude Code discovers project-level skills under `.claude/skills/`
automatically. To make the skill available in other repositories, copy or
symlink the directory into that repo's `.claude/skills/`, or into the user-level
`~/.claude/skills/` directory.
