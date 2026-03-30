---
description: Generate stable end-to-end tests for a real user workflow
agent: agent
argument-hint: scenario or target feature path
---

# Test: Generate End-to-End Tests

## Goal

Create reliable end-to-end tests that verify a complete user workflow through the UI using the project’s existing E2E framework.

## Inputs

- Target feature, route, or user workflow
- Existing E2E framework and conventions if known
- Any required seed data, auth state, fixtures, or environment assumptions

If the workflow is unclear, ask for the exact user journey rather than inventing one.

## Required workflow

1. Inspect the existing E2E setup, test style, helpers, fixtures, and naming conventions before writing tests.
2. Prefer the project’s current framework and folder structure:
   - Playwright if the workspace uses Playwright
   - Cypress if the workspace uses Cypress
3. Write tests around real user behavior, not implementation details.
4. Use stable locators in this order of preference:
   - accessible queries (`getByRole`, `getByLabel`, `getByText`)
   - existing semantic test helpers
   - `data-testid` only when necessary
5. Cover the primary success path and the most meaningful failure or edge state for the workflow.
6. Make the test deterministic:
   - avoid arbitrary sleeps
   - wait on user-visible state changes
   - use existing auth or seed helpers instead of hand-rolled setup
7. Verify the test against the existing suite conventions and note any environment prerequisites.

## Constraints

- Do not use brittle selectors such as CSS chains or XPath unless the project already depends on them and no stable alternative exists.
- Do not assert internal implementation details.
- Do not invent routes, copy, or UI states not supported by the code.
- Do not add a new E2E framework if the project already has one.
- Keep tests focused on the workflow, not exhaustive UI coverage.

## Output

Use this structure:

- **Workflow covered**
- **Test files created or updated**
- **Key assertions**
- **Environment assumptions**
- **Verification**

## Success criteria

- The test reflects a real user journey.
- The selectors and waits are stable and maintainable.
- The test fits the project’s existing E2E setup.
