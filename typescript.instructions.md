---
applyTo: "**"
---

# TypeScript Development Guidelines

## Project Context

- NestJS backend with ExpressJS, PostgreSQL database
- ReactJS frontend with NextJS, Tailwind CSS
- Strict TypeScript throughout. Zero `any` types allowed
- There are many helper methods/utilities in `src/helpers`. Always prefer them over new implementations! Prefetch all helpers before starting new code
- There are many predefined components in `src/components`. Always prefer them over new implementations! Prefetch all components before starting new code

## Core Rules

- **Never run/start applications** - server already running
- **Specify explicit return types** if not `void`, else not required
- **Use `for` loops instead of `forEach`** for performance
- **Return early** to reduce nesting depth
- **Keep functions under 20 lines** - extract logic when longer

## TypeScript Patterns

- **Use utility types**: `Partial<T>`, `Pick<T, K>`, `Omit<T, K>` over custom interfaces
- **Use `unknown` instead of `any`** for uncertain types
- **Add `as const` assertions** for immutable data
- **Include exhaustive `never` checks** in switch statements
- **Use `?.` optional chaining** and `??` nullish coalescing
- Prefer `interface` for object shapes and `type` for unions/utility compositions

## Code Organization

- **Use absolute imports** when available
- **Prefer named exports** over default exports
- **Use type-only imports** when importing only types
- **Name functions with verbs**, booleans with `is/has/can` prefixes
- **Avoid circular dependencies**
- **Never create index.ts files** - use explicit file names
- **Choose composition over inheritance**

## React/Frontend

- **Use functional components only** with proper TypeScript interfaces
- **Apply React.memo** when performance optimization needed
- **Extract reusable logic** into custom hooks
- **Implement error boundaries** with try/catch for async operations

## Testing (when requested)

- **Name test files**: `{module}.{action}.spec.ts` in `__tests__` subfolder
- **Focus on meaningful coverage** not percentages
- **Test core functionality** thoroughly

## Performance Requirements

- **Break down long operations** to prevent blocking
- **Use async/await consistently**
- **Apply destructuring** where it improves readability
