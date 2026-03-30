---
description: Generate focused unit tests for a defined source file or symbol
agent: agent
argument-hint: source file, symbol, or current selection
---

# Test: Generate Unit Tests

## Goal

Create behavior-focused unit tests for a specific file or symbol, covering happy paths, meaningful edge cases, and failure handling.

## Inputs

- Target source file, symbol, or current selection
- Existing test runner and conventions if known
- Any important domain rules or invariants

If no target is provided and there is no useful selection, ask for the exact file or symbol to test.

## Required workflow

1. Inspect the source file, its exported APIs, and neighboring tests before writing anything.
2. Determine the project’s existing test stack and conventions (Vitest, Jest, etc.) and follow them.
3. Define a compact test matrix:
   - expected behavior
   - edge cases
   - invalid input or error paths
   - observable side effects
4. Mock only true external boundaries such as network, filesystem, database, or time when needed.
5. Prefer clear arrange-act-assert structure and use table-driven tests when several cases share the same logic.
6. Keep tests deterministic, isolated, and readable.
7. Verify that the tests exercise behavior rather than duplicating implementation details.

## Constraints

- Do not test trivial assignments or framework internals.
- Do not invent behavior not present in the code.
- Do not over-mock internal logic that should be exercised directly.
- Restore or clear mocks consistently.
- Follow existing test file naming and placement conventions.

## Output

Use this structure:

- **Target under test**
- **Scenarios covered**
- **Test files created or updated**
- **Mocks and assumptions**
- **Verification**

## Success criteria

- The tests cover meaningful behavior and edge cases.
- The tests are deterministic and aligned with existing project patterns.
- The test suite becomes more useful, not noisier.
