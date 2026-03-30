---
description: Refactor a defined scope for clarity, maintainability, and safe performance wins
agent: agent
argument-hint: scope=path/to/file-or-folder or use current selection
---

# Refactor: Code

## Goal

Improve code quality in a clearly defined scope without changing intended behavior.

## Scope rules

- Refactor only the scope the user names.
- If no explicit scope is given, use the current selection or active file.
- If neither exists, ask for a file or folder instead of refactoring the entire workspace by default.
- Never expand scope beyond what is necessary to keep the target working.

## Required workflow

1. Inspect the target code, its direct dependencies, and nearby patterns before planning changes.
2. Identify concrete issues to address: duplication, unclear naming, long functions, nested logic, dead code, unsafe typing, or avoidable performance costs.
3. Make a short per-file plan before editing.
4. Refactor in small, behavior-preserving steps.
5. Prefer existing helpers, components, and abstractions over creating new ones.
6. Verify the touched area with the most relevant checks available.
7. Summarize what improved and any follow-up work that remains outside scope.

## Refactoring priorities

- Clear names and responsibilities
- Smaller functions and earlier exits
- Reduced nesting and duplication
- Safer typing and narrower contracts
- Simple performance wins such as removing repeated work or parallelizing safe async operations
- Modern TypeScript features when they improve correctness or readability

## Constraints

- Preserve current behavior unless the user explicitly asks for functional changes.
- Do not create new files unless clearly necessary for the refactor.
- Do not refactor unrelated code for “cleanup”.
- Do not add comments instead of improving the code itself.
- Do not optimize based on guesswork; change only what you can justify.

## Output

Use this structure:

- **Refactor scope**
- **Issues addressed**
- **Files changed**
- **Verification**
- **Follow-ups or trade-offs**

## Success criteria

- The code is easier to read and maintain.
- Behavior is preserved.
- The refactor stays tightly scoped and validated.
