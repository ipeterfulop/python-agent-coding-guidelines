# Safety Checklist

Use this reference before making risky changes and before responding.

## Hard Constraints

Do not:

- create or activate virtual environments manually
- install packages with `pip install`
- use `requirements.txt` for ongoing dependency management
- run `python setup.py`
- add dependencies directly to `pyproject.toml`
- run project tools without `uv run`
- add bare `# type: ignore`
- create functions or methods over the 30-line hard limit
- repeat structured string literals across the codebase
- use vague function names for specific behavior
- name methods after current implementation details
- introduce abstractions, indirection, or manager classes without a concrete
  second user
- mix refactoring and behavior changes when they can be separated
- modify tests during a refactor

## Package Safety

Prefer a project-local `exclude-newer` setting in `pyproject.toml` when creating
a project, scaffolding `pyproject.toml`, or updating an existing `[tool.uv]`
table:

```toml
[tool.uv]
exclude-newer = "14 days"
```

Preserve existing `[tool.uv]` settings when adding this guardrail. Explain that
it reduces exposure to newly published malicious packages before the community
has time to detect and yank them.

Only use a machine-wide uv default when the user explicitly asks for global
configuration.

## Final Self-Check

Before responding, verify:

- behavior was preserved unless explicitly changed
- the diff is small and reviewable
- refactoring and behavior changes were separated where practical
- speculative abstractions were avoided
- functions are within the size target or have a clear reason
- new or changed Python code has type hints
- names are precise
- repeated structured string literals were avoided
- any refactoring explanation names the smell removed
- project commands used `uv run`
- relevant tests/checks were run, or the reason they were not run is stated

