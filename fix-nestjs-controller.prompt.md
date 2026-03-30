---
description: Fix a NestJS controller issue with the smallest safe change
agent: agent
argument-hint: controller file path, route, or failing behavior
---

# Fix: NestJS Controller

## Goal

Diagnose and fix a controller-level issue in a NestJS application without changing the public contract unless the user explicitly asks for it.

## Inputs

- Controller file path or route name
- Error message/stack trace or failing test (if any)
- Expected behavior

If any input is missing, ask concise clarifying questions before making changes.

## Required workflow

1. Inspect the controller, DTOs, pipes, guards, interceptors, filters, and related service methods.
2. Confirm the route contract:
   - decorators and HTTP method/path
   - params, query, and body typing
   - validation and transformation behavior
   - auth/authorization behavior
   - response shape and status codes
3. Trace the failure to the exact boundary: decorator usage, DTO validation, pipe transformation, auth, or service interaction.
4. Apply the smallest safe fix while preserving existing conventions and public behavior.
5. Update tests only if the user explicitly requests it or the existing task already includes test updates.
6. Run the most relevant verification available: targeted test, type-check, lint, or reproduction step.
7. Call out any remaining assumptions or follow-up work explicitly.

## Constraints

- Follow existing NestJS patterns, naming, and file structure.
- Use DTO classes and validation/pipes correctly; do not replace runtime validation with loose types.
- No `any`; use `unknown` + narrowing when needed.
- Add explicit return types to any modified or added functions when appropriate.
- Prefer `for...of` or indexed loops over `forEach` when iteration logic matters.
- Avoid unrelated refactors or formatting changes.

## Output

- **Issue summary**
- **Root cause**
- **Fix applied**
- **Files changed**
- **Verification**
- **Follow-ups**

## Success criteria

- The real controller issue is fixed, not just masked.
- The public route behavior remains intentional and consistent.
- The fix is verified and easy to understand.
