---
agent: agent
---

# Fix a NestJS controller issue

## Inputs

- Controller file path or route name
- Error message/stack trace or failing test (if any)
- Expected behavior

If any input is missing, ask concise clarifying questions before making changes.

## Workflow

1. Inspect the controller, DTOs, guards, pipes, and related service methods.
2. Verify validation: DTOs are classes and ValidationPipe/Parse\* pipes are applied.
3. Apply the smallest safe fix without changing public API behavior.
4. Update tests only if the user explicitly requests it.

## Constraints

- Follow existing NestJS patterns, naming, and file structure.
- No `any`; use `unknown` + narrowing when needed.
- Explicit return types for all functions you add or modify.
- Prefer `for` loops over `forEach`.
- Avoid unrelated refactors or formatting changes.

## Output

- Summary of the issue and fix
- Files changed (one line each)
- Verification steps run (or explain why not run)
