---
description: Improve a component with clearer structure, accurate docs, and logic-focused tests
agent: agent
argument-hint: componentPath=src/components/YourComponent
---

# Refactor: Component

## Goal

Improve the target component by clarifying responsibilities, extracting reusable logic, adding accurate JSDoc where it helps, and strengthening logic-focused tests without changing the component’s intended behavior.

## Target

- Component folder: **${input:componentPath:src/components/<component-name>/}**
- If the user does not provide a path:
  - use the current file if it is already inside the target component folder
  - otherwise ask for the exact component folder path

## Required workflow

1. Inspect the component folder, neighboring components, existing helpers, and test patterns.
2. Identify what should stay in the component and what should move out:
   - pure business logic → `_helpers/`
   - stable constants/config → `_constants/`
   - meaningful presentational subparts → `_components/`
3. Create those folders only when there is real code to place in them; do not create empty structure for its own sake.
4. Remove inline comments that restate the obvious. Keep only comments that explain non-obvious business rules or constraints.
5. Add JSDoc to exported APIs and non-trivial helpers when it improves understanding.
6. Add or update logic-focused tests using the project’s existing test runner and conventions. Do not add UI rendering tests.
7. Verify imports, behavior, and tests after each meaningful refactor step.

## Reuse-first rules

- Search these locations before introducing new helpers/components, if they exist:
  - `src/common/**`
  - `src/components/**`
  - `src/modules/**`
- Prefer adapting an existing utility or component over creating a near-duplicate.

## TypeScript and React rules

- Strict TypeScript; no `any`.
- Use `unknown` + narrowing when needed.
- Add explicit return types to touched or added non-trivial functions.
- Use `for...of` or indexed loops instead of `forEach` when iteration logic matters.
- Prefer named exports.
- Do not create barrel files.
- Use modern TS features (`satisfies`, `as const`, discriminated unions, exhaustive `never` checks`) when they materially improve correctness.
- Keep props small, explicit, and readable.

## Testing rules

- Test only business logic, parsing, mapping, validation, or state derivation.
- Use simple deterministic tests with clear arrange-act-assert flow.
- Cover meaningful edge cases: empty input, nullish values, invalid input, and boundaries.
- Keep tests close to the extracted logic following project conventions.

## Constraints

- Do not change styling unless it is necessary to support the refactor.
- Do not rewrite unrelated files.
- Do not add README files or extra documentation beyond JSDoc.
- Do not add end-to-end or UI snapshot tests.
- Preserve the component’s public API unless a breaking change is explicitly requested.

## Output

- **Component scope**
- **Responsibilities extracted**
- **Comments/docs updated**
- **Tests added or updated**
- **Files changed**
- **Verification**
- **Follow-ups**

## Success criteria

- Responsibilities are better separated.
- The component is easier to read and reason about.
- Useful logic tests exist and no UI tests were added.
