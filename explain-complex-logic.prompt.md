# Explain: Complex Logic

**Goal**: Explain a complex piece of code in simple terms (ELI5 or Junior Dev level).

## Context
Useful for legacy code, complex algorithms, or regex patterns that are hard to decipher.

## Instructions

1.  **High-Level Summary**: "What does this block do in one sentence?"
2.  **Breakdown**: Go line-by-line or block-by-block.
3.  **Key Concepts**: Explain any advanced patterns used (e.g., Recursion, Memoization, Bitwise operations).
4.  **Data Flow**: Visualize how data transforms from input to output.

## Output Format

### 💡 Summary
This function calculates the user's credit score based on transaction history using a weighted moving average.

### 🔍 Detailed Breakdown
1.  **Lines 1-5**: Input validation. Checks if `history` array is empty.
2.  **Lines 8-12**: Filters out cancelled transactions.
3.  **Line 15**: `reduce()` function sums up the remaining values.

### 🧠 Concepts Used
*   **Array.reduce**: Used here to accumulate the total score.
*   **Guard Clause**: The early return on line 2 prevents errors.
