---
description: Improve a component with clean code and logic tests
agent: agent
argument-hint: componentPath=src/components/YourComponent
---

# Improve a component (Clean Code + Tests)

You are working in a strict TypeScript + React (Vite) codebase.

## Goal

Improve the target component by removing inline comments, adding high-quality JSDoc, applying Clean Code by extracting logic into dedicated folders, and adding unit tests for business logic (no UI tests).

## Target

- Component to improve: **${input:componentPath:src/components/<component-name>/>}**
  - If not provided, ask for the exact component folder path.

## Requirements

### 1) Remove inline comments

- Remove inline comments that explain _what the code already says_.
- Keep only comments that explain non-obvious business rules.
- Prefer self-documenting names over comments.

### 2) Add JSDoc (developer-facing documentation)

- Add JSDoc to:
  - exported functions
  - exported types
  - complex private helper functions (only when behavior is non-trivial)
- JSDoc must describe:
  - purpose/intent
  - parameters and return value
  - edge cases / invariants
  - errors thrown (if any)
- Keep JSDoc accurate and concise; do not document React props that are already obvious unless there are non-trivial constraints.

### 3) Apply Clean Code via extraction

Refactor the component folder to separate responsibilities:

- `component/_helpers/`
  - pure, testable business logic (parsing, mapping, filtering, transformations, validation)
  - must not import React
  - must not depend on UI components

- `component/_constants/`
  - constants, default values, static maps, stable labels/keys
  - prefer `as const` and `satisfies` where appropriate

- `component/_components/`
  - presentational subcomponents or small UI building blocks
  - keep props small and explicit

Constraints:

- Keep functions small (aim < 20 lines); extract when longer.
- Return early to reduce nesting.
- Prefer composition over inheritance.
- Maintain existing public API unless the change is clearly safe and required.

Reuse-first constraint:

- Before creating new helpers/components, search for existing implementations in:
  - `src/common/**`
  - `src/components/**`
  - `src/modules/**`
- Prefer reusing existing utilities/components over introducing new ones.

### 4) Add tests for business logic only (no UI tests)

- Add/extend tests for the extracted business logic.
- Place tests in:
  - `component/__tests__/` (tests for component-level logic that is not UI rendering)
  - `component/_helpers/__tests__/` (unit tests for helper functions)

Testing constraints:

- **No UI tests**: do not mount the component, do not snapshot, do not assert DOM.
- Keep tests simple and deterministic; prefer unit tests for pure functions.
- Cover edge cases (empty input, undefined/null where relevant, invalid values, boundary conditions).
- Use the project’s existing test patterns (Vitest).

## TypeScript / Codebase Rules (must follow)

- Strict TypeScript: **no `any`**.
- Use `unknown` + narrowing when needed.
- Add **explicit return types** for all functions you touch or add.
- Use `for` loops instead of `forEach`.
- Prefer named exports.
- No re-exports allowed (no barrel exports):
  - do not create `index.ts` files
  - do not add `export * from ...` / re-export-only modules
- Use modern TS features when they improve correctness/readability:
  - `satisfies` for config objects
  - `as const` for immutable constants
  - discriminated unions where applicable
  - exhaustive `never` checks in switches

## “Do not do” list

- Do not add or change UI styling unless strictly necessary.
- Do not rewrite unrelated files.
- Do not add README or documentation files.
- Do not add end-to-end tests.

## Step-by-step instructions

1. Inspect the component folder and identify:
   - inline comments to remove
   - business logic candidates for extraction
   - constants candidates for extraction
   - UI subparts that can become subcomponents
2. Perform the refactor in small steps, ensuring builds/tests still pass.
3. Move business logic into `_helpers` and constants into `_constants`.
4. Update imports and ensure no React imports end up in `_helpers`.
5. Add/adjust JSDoc as specified.
6. Add unit tests for extracted logic.
7. Ensure lint/typecheck/tests pass.

## Success criteria

- Inline comments removed (except truly necessary business-rule clarifications).
- JSDoc added to exported APIs and non-trivial helpers.
- Component responsibilities are split across `_helpers`, `_constants`, `_components` appropriately.
- Added/updated unit tests cover business logic and edge cases; **no UI tests added**.
- No TypeScript errors; no `any` introduced.

## Optional improvements (only if clearly beneficial)

- Replace “boolean soup” parameters with a single options object typed via `satisfies`.
- Extract duplicated logic into a single helper.
- Replace stringly-typed keys with string literal unions derived from `as const` maps.
- Add small type guards for unsafe inputs.
