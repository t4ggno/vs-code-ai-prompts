---
description: Design a feature and scaffold the minimal file structure needed to implement it
agent: agent
argument-hint: feature goal, target area, or folder path
---

# Design: Scaffold Feature

## Goal

Translate a feature request into a concrete, codebase-aligned design and scaffold the smallest sensible set of files, types, and interfaces.

## Inputs

- Feature goal and primary user flow
- Target area or folder if known
- Constraints such as framework, API contract, auth rules, design system, or performance requirements
- Current selection or files that must be extended

If scope is ambiguous, ask focused questions only when the answer would materially change the architecture. Otherwise, state the assumption and proceed.

## Required workflow

1. Inspect relevant existing folders, naming patterns, and neighboring features before proposing structure.
2. Break the request into responsibilities: UI, state, domain logic, validation, data access, tests, and integration points.
3. Propose the minimal architecture that fits the codebase. Do not create speculative layers.
4. Define the file and folder plan with a one-line purpose for each file.
5. Define the key contracts first: props, DTOs, service signatures, types, interfaces, and events.
6. Scaffold only what is necessary:
   - If the user asked for planning only, stop after the design.
   - If the user asked for scaffolding or implementation, create minimal, compile-friendly stubs that follow existing patterns.
7. Call out unanswered questions or deferred decisions explicitly.

## Constraints

- Reuse existing helpers, components, and patterns before creating new ones.
- Preserve existing architecture and naming conventions.
- Do not invent APIs or data fields without marking them as assumptions.
- Avoid empty placeholder files that add no value.
- Keep public interfaces explicit and minimal.

## Output

Use this structure:

- **Feature summary**
- **Assumptions**
- **Proposed structure**: file tree plus one-line purpose per file
- **Key contracts**: main types, interfaces, props, DTOs, and functions
- **Scaffold status**: planned only or files created
- **Open questions / follow-ups**

## Success criteria

- The design fits the existing codebase.
- The scaffold is minimal, coherent, and directly usable.
- The next implementation steps are obvious.
