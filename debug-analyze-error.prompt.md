---
description: Diagnose an error, identify the root cause, and implement or recommend the smallest safe fix
agent: agent
argument-hint: error details, stack trace, failing command, or failing test
---

# Debug: Analyze Error and Fix

## Goal

Diagnose a coding error or runtime failure, identify the real root cause, and either implement the smallest safe fix or explain the precise change needed.

## Inputs

- Error message, stack trace, failing command, or failing test output
- Relevant file, symbol, or code snippet
- Reproduction steps or observed vs expected behavior

If a required input is missing and cannot be inferred from the workspace or current selection, ask only the minimum clarifying questions needed to continue.

## Required workflow

1. Inspect the failing file, surrounding code, and relevant config/tests before drawing conclusions.
2. Reconstruct the failure path and identify the exact condition that breaks.
3. Explain the root cause in plain language with concrete evidence from the code or error output.
4. Choose the smallest safe fix that preserves intended behavior and matches existing conventions.
5. If the user asked for a fix, implement it directly. If the user asked for analysis only, do not edit files.
6. Run the most relevant verification available: reproduction step, targeted test, type-check, lint, or diagnostics.
7. Call out any remaining risks, edge cases, or missing context explicitly.

## Constraints

- Do not guess about code you have not inspected.
- Do not mask the problem with broad try/catch blocks, silent fallbacks, or unsafe type escapes unless clearly justified.
- Avoid unrelated refactors.
- Prefer fixing the underlying cause over patching the symptom.
- If more than one plausible cause remains, rank them by confidence and explain what would confirm each one.

## Output

Use this structure:

- **Problem summary**: what failed and where
- **Root cause**: precise explanation with evidence
- **Fix applied / recommended**: minimal change and why it works
- **Verification**: what was checked and the result
- **Follow-ups**: remaining risks, edge cases, or open questions

## Success criteria

- The explanation identifies the actual cause, not a guess.
- The fix is minimal, grounded in evidence, and verified.
- The user can clearly understand what failed, why, and how it was resolved.
