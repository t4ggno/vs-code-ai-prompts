---
description: Refactor a defined scope for clarity, maintainability, and safe performance wins
agent: agent
argument-hint: scope=path/to/file-or-folder, use current selection, or omit scope for a full workspace pass
---

# Refactor: Code

## Goal

Improve code quality in a clearly defined scope, or across the full workspace when no narrower scope signal is provided, without changing intended behavior.

## Scope rules

- Refactor only the scope the user names.
- If no explicit scope is given, use attached files, the current selection, or the active file when any of them provide a clear target.
- If the user does not define a scope and there are no attached files, current selection, or active file that clearly define the target, treat the entire workspace as the scope.
- In full-workspace mode, first create a file-by-file plan that accounts for every file in the workspace.
- In full-workspace mode, files may be marked as **optimized**, **no changes needed**, **skipped** (with reason), or **blocked** (with reason), but no file should be left unaccounted for.
- Generated files, dependencies, binaries, build artifacts, lockfiles, and other files that should not be refactored may be skipped, but they must still be listed in the plan with a short reason unless the user explicitly included them.
- Never expand scope beyond what is necessary to keep the target working.

## Required workflow

1. Determine the actual scope first and state whether you are running in focused mode or full-workspace mode.
2. Inspect the target code, its direct dependencies, and nearby patterns before planning changes.
3. In full-workspace mode, enumerate the workspace files and create a short per-file checklist before editing.
4. Identify concrete issues to address: duplication, unclear naming, long functions, nested logic, dead code, unsafe typing, or avoidable performance costs.
5. Make a short per-file plan before editing.
6. Refactor in small, behavior-preserving steps.
7. After each file, update the checklist to mark it as optimized, no changes needed, skipped, or blocked, then continue to the next file until every file in scope has been accounted for.
8. Prefer existing helpers, components, and abstractions over creating new ones.
9. Verify the touched area with the most relevant checks available.
10. Summarize what improved and any follow-up work that remains outside scope.

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
- **File-by-file plan**
- **Issues addressed**
- **Files changed**
- **Verification**
- **Follow-ups or trade-offs**

## Success criteria

- The code is easier to read and maintain.
- Behavior is preserved.
- If a narrow scope was provided or inferred, the refactor stays tightly scoped and validated.
- If full-workspace mode was used, every file in the workspace was accounted for and marked as optimized, no changes needed, skipped, or blocked before stopping.
