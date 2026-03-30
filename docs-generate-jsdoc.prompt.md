---
description: Add accurate JSDoc or TSDoc comments to existing code without changing behavior
agent: agent
argument-hint: selection, symbol, or file path
---

# Docs: Generate JSDoc or TSDoc

## Goal

Add or improve documentation comments that explain intent, contracts, side effects, and important constraints without changing implementation behavior.

## Inputs

- Target file, symbol, or current selection
- Language and context (TypeScript, JavaScript, React, NestJS, etc.)
- Any domain rules the comments must reflect

If no target is given and there is no useful current selection, ask for the exact file or symbol to document.

## Required workflow

1. Read the target code and enough surrounding context to understand what it actually does.
2. Document only behavior that is supported by the code or explicit user context.
3. Add or update comments only where they add value:
   - exported functions, classes, hooks, and types
   - complex internal helpers with non-obvious behavior
   - constraints, invariants, or side effects that are easy to miss
4. Use the most appropriate style for the file:
   - TSDoc for TypeScript APIs
   - JSDoc for JavaScript or mixed files
5. Include tags only when they are true and useful (`@param`, `@returns`, `@throws`, `@example`, `@deprecated`, etc.).
6. Preserve the code logic exactly; change comments only unless a tiny signature alignment is required for documentation accuracy.

## Constraints

- Do not invent behavior, exceptions, or examples.
- Do not restate the obvious line by line.
- Do not change runtime logic to make the docs look cleaner.
- Keep comments concise, accurate, and maintainable.

## Output

Use this structure:

- **Scope documented**
- **Comments added or updated**
- **Files changed**
- **Notes**: ambiguity, assumptions, or APIs that still need domain clarification

## Success criteria

- Comments explain intent and constraints, not just syntax.
- Documentation matches the real code.
- No implementation behavior is changed accidentally.
