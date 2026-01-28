# Debug: Analyze Error & Fix

**Goal**: Diagnose a coding error or runtime exception and propose a fix.

## Context
When investigating bugs, we need to trace the root cause, not just patch the symptoms.

## Instructions

1.  **Input Data**: Ask for the following if not provided:
    *   Error message / Stack trace.
    *   Relevant code snippet(s).
    *   Steps to reproduce.
2.  **Analysis Phase**:
    *   Trace the execution flow leading to the error.
    *   Identify the exact line or logic failure.
    *   Explain *why* it failed (e.g., "The variable `user` is undefined because the API call is async and wasn't awaited").
3.  **Solution Phase**:
    *   Propose a fix.
    *   Explain any side effects of the fix.
    *   Suggest a test case to prevent recurrence.

## Output Format

### 🚨 Root Cause Analysis
[Explanation of what went wrong]

### 🛠️ Proposed Fix

```typescript
// Old Code
const data = fetchData(); // missing await

// New Code
const data = await fetchData();
```

### ✅ Verification
Add this test case to verify the fix:
```typescript
it('should handle async fetch correctly', async () => { ... })
```
