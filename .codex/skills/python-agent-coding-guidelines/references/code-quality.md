# Code Quality

Use these rules when editing Python code.

## Core Rules

- Preserve existing behavior unless explicitly asked to change it.
- Prefer small, reviewable diffs.
- Use the existing project style where it is reasonable.
- Improve low-quality patterns in files already being edited.
- Do not optimize for cleverness or compactness.
- Do not hide complexity behind vague helper names.
- Do not leave newly added code without type hints.

## Function And Method Size

Target functions and methods at 20 lines or fewer.

Hard limit: do not exceed 30 lines. If a function grows beyond that, decompose it
along responsibility boundaries.

Each function should have:

- one responsibility
- clear inputs
- clear outputs
- a precise name

Use guard clauses instead of deeply nested branches. Avoid nesting deeper than
three levels unless there is a strong local reason.

## Naming

Names must describe exact behavior.

Avoid vague names such as:

```python
update_changes()
process_data()
handle()
do_work()
prepare()
```

Prefer precise names such as:

```python
update_person_name_and_age()
calculate_invoice_total_with_tax()
load_user_profile_from_database()
build_customer_search_query()
parse_snowflake_result_rows()
```

If a function updates a few fields, name those fields in the function name.

Name methods after what they mean, not what they currently do. A method name
should sit one level of abstraction above the implementation. Class names should
name what the thing is, not a speculative generalization.

The same argument name across functions should refer to the same concept. If
`number` means different things in different places, rename one of them.

Use descriptive variables. Avoid `data`, `tmp`, `val`, `res`, `obj`, and `item`
unless the scope is very small and obvious.

If the right name is not obvious:

1. Spend a few minutes looking for the best name.
2. Use an obvious placeholder such as `_unnamed_concept` with a `# FIXME:
   rename` comment.
3. Ask the user for the domain term.

## String Literals And Field Constants

Avoid repeated raw string literals for structured concepts:

- database column names
- table field names
- JSON keys
- API payload keys
- dataframe columns
- Snowflake result fields

Centralize repeated structured field names in a frozen dataclass:

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class UserFields:
    NAME: str = "name"
    AGE: str = "age"
```

Use names like `{ModelName}Fields` or `{ModelName}Attributes`.

Do not create a constants class for a literal used once in a local context.

## Python Idioms

Use Python idioms when they sharpen intent. Fall back to plain `for` loops and
explicit `try` / `finally` when an idiom would reduce clarity.

Use comprehensions when all are true:

- the output is a collection built from an iterable
- the body is one expression
- there is at most one simple filter
- there are no side effects
- the line remains readable under the project line length

Prefer a plain `for` loop when the body mutates external state, performs I/O,
has multiple filters, has nested conditional loops, or would need a comment to
explain it.

Use generators when data is large, unbounded, streamed, consumed one item at a
time, or expensive to materialize. Return a list or tuple when callers need
length, indexing, slicing, repeated iteration, or predictable resource cleanup.

Generator classes are rarely the right tool. Reach for one only when iteration
state must coexist with other methods or attributes callers use during
iteration. Otherwise, write a generator function.

Use context managers for files, sockets, locks, database connections, cursors,
transactions, temporary state, timing, tracing, and profiling. Do not manually
pair `open()` / `close()`, `acquire()` / `release()`, or `begin()` / `commit()`
when a context manager exists.
