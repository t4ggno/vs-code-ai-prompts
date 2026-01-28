# Test: Generate Unit Tests (Jest/Vitest)

**Goal**: Create comprehensive unit tests for a specific file, covering happy paths, edge cases, and error states.

## Context
Robust unit tests prevent regression. This prompt guides the generation of tests that verify behavior, not just implementation details.

## Instructions

1.  **Analyze the Source File**: Understand imports, exported functions, and dependencies.
2.  **Plan Test Cases**:
    *   **Happy Path**: Verify standard usage returns expected results.
    *   **Edge Cases**: Test empty inputs, `null`, `undefined`, boundary numbers (0, -1, MaxInt).
    *   **Error Handling**: Verify that errors are thrown or caught as expected.
3.  **Mock Dependencies**:
    *   Use `jest.mock()` or `vi.mock()` for external modules.
    *   Mock database calls, API requests, and standard I/O.
4.  **Write Tests**:
    *   Use `describe()` blocks to group tests by function/class.
    *   Use `it()` or `test()` for individual cases with descriptive names.
    *   Use `expect()` assertions.
    *   Follow the **AAA** pattern: **Arrange** (setup), **Act** (execute), **Assert** (verify).

## Constraints
- Do not test trivial implementation details (e.g., "it should assign a variable").
- Ensure all mocks are cleared/restored (`beforeEach`).
- Use table-driven tests (`test.each`) for repetitive cases.

## Output Format

```typescript
import { functionName } from './source-file';
import { dependency } from './dependency';

vi.mock('./dependency');

describe('functionName', () => {
  beforeEach(() => {
    vi.clearAllMocks();
  });

  it('should return result for valid input', () => {
    // Arrange
    vi.mocked(dependency).mockReturnValue(true);
    
    // Act
    const result = functionName('input');

    // Assert
    expect(result).toBe('success');
    expect(dependency).toHaveBeenCalledWith('input');
  });

  it('should throw error for invalid input', () => {
    expect(() => functionName('')).toThrow('Invalid input');
  });
});
```
