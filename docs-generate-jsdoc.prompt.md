# Docs: Generate JSDoc/TSDoc Comments

**Goal**: Add comprehensive, standard-compliant JSDoc/TSDoc comments to functions, classes, and interfaces.

## Context
Clear documentation is essential for maintainability and IDE support. This prompt guides the AI to add documentation that explains *why* code exists, not just *what* it does.

## Instructions

1.  **Analyze the Selection**: Read the selected code to understand its purpose, parameters, return values, and side effects.
2.  **Generate Documentation**: Create JSDoc/TSDoc comments for:
    *   Classes: Description of responsibility.
    *   Methods/Functions: Description, `@param`, `@returns`, `@throws`, and `@example`.
    *   Interfaces/Types: Description of the data structure.
3.  **Adhere to Standards**:
    *   Use Markdown for formatting within comments.
    *   Do not state the obvious (e.g., "Returns string" for a function named `getString`). Focus on business logic or constraints.
    *   Include `@deprecated` if applicable.
4.  **Preserve Code**: Do not modify the actual code implementation, only add/update comments.

## Output Format

```typescript
/**
 * [Short description of what the function does]
 *
 * [Optional: Detailed explanation or context]
 *
 * @param {Type} paramName - [Description of the parameter]
 * @returns {Type} [Description of the return value]
 * @throws {ErrorType} [Condition that causes the error]
 *
 * @example
 * // [Description of example]
 * const result = functionName(arg);
 */
export function functionName(paramName: Type): Type {
    // ...
}
```

## Example Usage

**Input:**
```typescript
function calculateTax(amount: number, region: string) {
  // ...
}
```

**Output:**
```typescript
/**
 * Calculates the total tax for a transaction based on the region's tax rate.
 *
 * Handles special tax exemptions for non-profit regions.
 *
 * @param {number} amount - The base transaction amount in cents.
 * @param {string} region - The ISO 3166-2 region code (e.g., 'US-NY').
 * @returns {number} The calculated tax amount, rounded to the nearest cent.
 * @throws {InvalidRegionError} If the region code is not supported.
 */
function calculateTax(amount: number, region: string) {
  // ...
}
```
