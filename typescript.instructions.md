---
applyTo: "**"
---

# TypeScript Development Guidelines

## Applicability

- Apply these rules when the relevant scope contains TypeScript or JavaScript, or when the user asks for NestJS, React, or Next.js work.
- If the workspace is not TypeScript-based, fall back to the general instructions and keep only the rules that still make sense.

## Project context

- Common stack: NestJS + Express + PostgreSQL backend, React + Next.js + Tailwind frontend.
- Assume strict TypeScript unless the project clearly uses a different setup.

## Investigate first

- Before adding new code, inspect nearby modules and search for reusable helpers or components.
- If `src/helpers` exists, check it before introducing new helpers.
- If `src/components` exists, check it before creating new UI components.
- Prefer project-defined scripts and config over guessed commands.

## Core Rules

- Do not start long-running app servers unless the user explicitly asks.
- Add explicit return types for public functions and any non-trivial new functions.
- Prefer early returns to reduce nesting.
- Aim to keep functions small and single-purpose; extract logic when length obscures intent.
- Avoid `any`; use `unknown`, generics, unions, or type guards instead.
- Prefer `for...of` or indexed loops over `forEach` when you need predictable control flow or hot-path performance.

## TypeScript Patterns

- Prefer utility types such as `Partial`, `Pick`, `Omit`, `Required`, and `Record` when they improve clarity.
- Use `as const`, `satisfies`, discriminated unions, and exhaustive `never` checks where they strengthen correctness.
- Use `?.` optional chaining and `??` nullish coalescing for null-safe code when appropriate.
- Prefer `interface` for extensible object contracts and `type` for unions or utility compositions.
- Use type-only imports when possible.

## Code Organization

- Prefer named exports unless the surrounding module pattern clearly uses default exports.
- Use absolute imports when the project is configured for them.
- Avoid circular dependencies and barrel files unless the codebase already depends on them.
- Name functions with verbs and booleans with `is`, `has`, or `can` prefixes.
- Choose composition over inheritance.

## React/Frontend

- Use functional components and typed props.
- Extract reusable logic into hooks when it reduces duplication.
- Handle loading, empty, error, and disabled states when relevant.
- Keep server and client boundaries correct in Next.js and avoid mixing concerns.

## Testing (when requested)

- Follow the project’s existing test framework and file naming conventions.
- Prefer meaningful behavior coverage over testing implementation details.
- Add tests only when requested or when the active task explicitly includes them.

## Validation

- Prefer targeted lint, type-check, and test commands defined in the project over guessed commands.
- Do not change code just to silence types or lints without understanding the cause.
