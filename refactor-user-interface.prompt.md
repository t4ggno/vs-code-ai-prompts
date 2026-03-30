---
description: Improve a UI surface with clear, codebase-aligned UX refinements
agent: agent
argument-hint: scope=path/to/view-or-component or use current selection
---

# Refactor: User Interface

## Goal

Improve the visual clarity, usability, and consistency of an existing UI without changing backend behavior or expanding the feature scope.

## Scope rules

- If the user specifies a path or view, limit changes to that scope.
- If no explicit scope is given, use the current selection or active view instead of redesigning the entire app by default.
- Only improve all views when the user explicitly asks for an app-wide UI pass.

## Inputs

- Target view, component, or current selection
- Optional screenshots, brand/tone constraints, or UX references
- Primary user task for the surface, if known

If the intended UX is unclear from code alone, request screenshots or one short clarifying answer. Otherwise, proceed with reasonable, stated assumptions.

## Required workflow

1. Inspect the existing UI code, shared components, design primitives, and styling patterns.
2. Identify the most impactful usability issues in the target scope: hierarchy, spacing, readability, empty states, loading states, responsiveness, or action clarity.
3. Prefer reusing and composing existing design-system or shadcn-style components over creating one-off UI.
4. Keep changes intentional and reversible: improve layout, copy, states, and interaction polish before inventing new patterns.
5. Verify accessibility basics in the touched area: focus states, semantic structure, tap targets, contrast, and keyboard flow.
6. Keep the data flow and routing intact unless the user explicitly requests broader changes.

## Constraints

- Do not redesign unrelated screens.
- Do not change backend contracts or remove existing functionality.
- Do not introduce new dependencies unless explicitly requested.
- Do not over-style or add decorative noise that hurts clarity.
- Do not ask for screenshots if the UI intent is already clear from the code.

## Output expectations

- **UI scope**
- **Problems improved**
- **Files changed**
- **Verification**
- **Assumptions / follow-ups**

## Success criteria

- The target UI is easier to scan and use.
- The changes fit the existing design system.
- Accessibility and responsiveness remain intact.
