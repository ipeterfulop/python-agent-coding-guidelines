# Refactoring

Use this reference when restructuring Python code, especially legacy or
AI-generated code.

## Refactoring Discipline

Refactoring changes code arrangement without changing behavior.

Do not mix "make room" and "add feature" in the same diff when they can be
separated. Before adding behavior, ask:

1. Is the code already open to this change?
2. If not, what refactor would make the new behavior straightforward?

During a refactor:

- change in small steps
- run tests after meaningful steps when feasible
- do not modify tests during the refactor
- if tests fail, make a smaller change
- name the smell being removed in the explanation or commit message

## Smells To Recognize

| Smell | Symptom | Typical cure |
|---|---|---|
| Duplicated Code | Same logic in multiple places | Extract a function after the duplication has a clear name |
| Long Method | Function or method exceeds the 30-line hard limit | Extract helpers along responsibility lines |
| Long Parameter List | Four or more positional params, or unrelated params | Group into a dataclass or object |
| Switch Statements | Branching on type, kind, or status | Polymorphism, dispatch dict, or one class per case |
| Primitive Obsession | Domain concepts modeled as `int`, `str`, `dict`, or `tuple` | Extract a value object or frozen dataclass |
| Data Clump | Same three or more values travel together | Group into one object |
| Feature Envy | A method uses another object's data more than its own | Move behavior to the data owner |
| Shotgun Surgery | One logical change requires edits across many files | Consolidate the missing abstraction |
| Divergent Change | One module changes for many unrelated reasons | Split along change axes |
| Speculative Generality | Abstraction with no second user | Delete it and restore concrete code |
| Inappropriate Intimacy | Modules know each other's internals | Push behavior into the owner |
| Comments as Deodorant | Comment explains what the code does | Rename or extract until the comment is unnecessary |

## Shameless Green

Prefer the simplest correct solution, even with some duplication, until a
concrete change request justifies abstraction.

Avoid:

- incomprehensibly concise code
- speculative generality
- concretely abstract methods named after current implementation details

Introduce an abstraction only when there is a second concrete change request that
touches the same logic, or a third near-identical occurrence sharing one reason
to change.

## OOP Guidance

Do not force OOP everywhere. Use OOP when it improves cohesion, encapsulation,
reuse, testability, or clarity of domain behavior.

Good OOP candidates:

- related operations over the same state
- domain concepts with behavior
- repeated workflows with shared dependencies
- services coordinating external systems
- validation and transformation logic that belongs together

Prefer dataclasses for structured data and frozen dataclasses for immutable
config, field constants, and value objects.

Do not:

- create classes only to group unrelated functions
- create large manager classes with mixed responsibilities
- hide procedural code inside a class without improving structure
- mix unrelated concerns in the same class

## Extract Class Diagnostic

Extracting a class is likely justified when three or more are true:

1. Several methods have the same shape.
2. Those methods take an argument with the same name and concept.
3. The argument carries most of the logic.
4. A private/public line would put the same methods together.
5. The argument is a primitive standing in for a domain concept.

Safe extract-class sequence:

1. Create the new class.
2. Copy the relevant methods without deleting the originals.
3. Use the new class at tested call sites.
4. Once green, delete the unused originals.
