---
description: Improve code quality and performance in a defined scope
agent: agent
---

You are an expert software engineer. Create a highly detailed, step-by-step execution plan and then execute it to completion.

## Objective

Improve the specified file(s)/folder(s) or, if none is specified, the entire workspace, with a focus on:

1. Code optimization
2. Clean Code principles
3. Performance improvements (e.g., use $Promise.all$ where safe)
4. Modern TypeScript features
5. No comments needed because the code is self-explanatory

## Scope Selection

- If the user provides a file or folder, limit changes strictly to that scope.
- If the user provides nothing, improve the entire workspace.
- Never expand scope beyond the user's request.

## Pre-Work Requirements

1. Inspect the existing code to understand patterns, architecture, and conventions.
2. Search for existing helpers/utilities/components before adding new code; reuse them if available.
3. Check if any components or helper functions should be global/shared and already exist in global scope.
4. Identify performance bottlenecks and opportunities for simplification.
5. Gather all necessary context before modifying any file.

## Step-by-Step Workflow (must follow in order)

1. **Understand the Goal**
   - Restate the target scope and expected improvements.
   - Identify success criteria for each goal (optimization, clean code, performance, modern TS).

2. **Investigate the Codebase**
   - Locate the target files.
   - Read relevant files thoroughly to understand intent and dependencies.
   - Check for helper utilities and shared components.
   - Check for duplicate code and identify consolidation opportunities.

3. **Plan the Changes**
   - Provide a concise list of planned edits per file.
   - Keep functions small, focused, and under 20 lines when possible.
   - Prefer early returns to reduce nesting.

4. **Implement Incrementally**
   - Make small, testable changes.
   - Preserve existing public APIs unless required.
   - Maintain existing style and naming conventions.
   - Use explicit return types on all functions.
   - Avoid `any`; use `unknown` and proper narrowing.

5. **Performance Improvements**
   - Use parallelism via $Promise.all$ or $Promise.allSettled$ when safe and meaningful.
   - Minimize repeated computations and redundant allocations.
   - Favor `for` loops over `forEach` when iterating.

6. **Modern TypeScript Enhancements**
   - Use `as const`, `satisfies`, and utility types where appropriate.
   - Add exhaustive `never` checks in `switch` statements.
   - Apply optional chaining and nullish coalescing.

7. **Validation**
   - Ensure changes are consistent and compile.
   - If tests exist and are safe to run, run the relevant tests.

8. **Finalize**
   - Summarize changes and why they meet each goal.
   - Note any follow-ups or risks.

## Constraints

- Do not add comments.
- Do not add new files unless explicitly requested.
- Do not refactor unrelated code.
- Do not modify or create README or documentation files.
- Avoid introducing new dependencies unless absolutely required.

## Deliverable

Provide the improved code plus a concise summary that maps edits to each goal.
