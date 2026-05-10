# uv Workflow

Use this reference for Python project commands and package operations.

## Toolchain

`uv` is the package and environment manager.

Do not use:

- `pip`
- `pip-tools`
- `poetry`
- `conda`
- manually created virtual environments
- manually activated virtual environments

Do not run project tools directly when they should resolve through the project
environment.

## Package Operations

| Intent | Command |
|---|---|
| Add a runtime dependency | `uv add <package>` |
| Add a dev-only dependency | `uv add --dev <package>` |
| Remove a dependency | `uv remove <package>` |
| Sync environment to lockfile | `uv sync` |
| Regenerate lockfile | `uv lock` |

Modify dependencies through `uv add` or `uv remove`, not by hand-editing
dependency lists in `pyproject.toml`.

## Running Python And Tools

Use `uv run` for project execution:

```bash
uv run python script.py
uv run python -m module_name
uv run pytest
uv run ruff check .
uv run ruff format .
uv run mypy .
```

Use `uvx` only for one-off tools that are not project dependencies.

## Project Initialization

For an application or script:

```bash
uv init project-name
```

For a distributable library:

```bash
uv init --package project-name
```

Do not create `setup.py`, `setup.cfg`, or `requirements.txt` unless the user
explicitly asks for compatibility files.

## Testing

Use pytest.

Defaults:

- entry point: `uv run pytest`
- test root: `tests/`
- test files: `test_*.py`
- test functions: `test_*`

Useful commands:

```bash
uv run pytest
uv run pytest tests/test_api.py
uv run pytest tests/test_api.py::test_specific_case
uv run pytest -v -x -s
uv run pytest --cov=mypackage --cov-report=term-missing
```

Tests should assert what the code does, not how it does it.

## Linting And Formatting

Use ruff:

```bash
uv run ruff check .
uv run ruff check --fix .
uv run ruff format .
uv run ruff format --check .
```

Configuration should live under `[tool.ruff]` in `pyproject.toml`.

## Type Checking

Prefer `ty` when configured. Fall back to `mypy`.

```bash
uv run ty check
uv run mypy .
```

Add type hints to function arguments, return values, useful class attributes,
dataclasses, and typed structures.

Never add a bare `# type: ignore`. Use a specific error code and reason where
useful.

## Pre-commit

Prefer `prek`; use `pre-commit` as an alternative. Install with `uvx`, never
`pip`.

```bash
uvx prek install
uvx pre-commit install
```
