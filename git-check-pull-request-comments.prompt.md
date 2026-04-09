---
description: Validate PR review comments, apply justified changes, and prepare clear resolution notes
agent: agent
---

# PR Review Comment Triage and Fixes

## Goal

Thoroughly review the **currently active pull request** and its **review comments**, validate whether each comment is correct, and apply the clearly justified changes needed to move the PR toward approval.

Before classifying comments, silently ignore automated comments from coverage or code-analysis tools (for example Codecov, Codacy, or similar bots). Do not include them in summaries, tables, or resolution notes unless the user explicitly asks.

## Decision rubric

For each review comment or thread, determine:

- **Type**: bug, correctness, security, performance, maintainability, style, DX, tests, or docs
- **Validity**: correct, partially correct, incorrect, outdated, or unclear
- **Severity**: blocking, non-blocking, or nit
- **Action**: fix now, partially address, reject with rationale, or ask for clarification

## Required workflow (do not skip)

1. **Load PR context first**
   - Retrieve the active pull request details (title, description, changed files, review comments, checks).
   - If there is no active PR, fall back to the currently open PR.
   - If no PR is available, explain that and stop.

2. **Read + classify every review comment**
   - First, filter out automated comments from coverage/code-analysis tools (such as Codecov, Codacy, and similar bots) without mentioning that filtering in the output.
   For each comment/thread, determine:
   - **Type**: bug, correctness, security, performance, maintainability, style, DX, tests, docs.
   - **Validity**: correct / partially correct / incorrect / outdated.
   - **Severity**: blocking / non-blocking / nit.
   - **Action**: fix now / propose alternative / explain why not / ask for clarification.

3. **Validate comment rationale**
   - Cross-check against the codebase: read the referenced files and surrounding context.
   - Ensure suggestions match existing project conventions and architecture.
   - Reject suggestions that:
     - break public APIs without justification,
     - introduce `any` types,
     - add unnecessary abstractions,
     - conflict with established utilities/components.

4. **Apply necessary + useful changes**
   - Implement only changes that are clearly beneficial and aligned with comments.
   - Keep changes small and incremental; avoid unrelated refactors.
   - Prefer existing utilities/components in `src/helpers` and `src/components` over new ones.
   - Add/adjust code in the minimal set of files needed.

5. **Verify**
   - Run the relevant checks (lint/test/build as appropriate).
   - Fix any new errors introduced by your changes.

6. **Prepare reviewer-friendly resolution notes**
   - Provide a concise summary that maps **comment → decision → change**.
   - If you disagree with a comment, explain briefly and cite code evidence.
   - Include “follow-up suggestions” that are helpful but non-blocking.
   - Do **not** post review replies, resolve threads, or submit a review unless the user explicitly asks.

## Constraints

- Follow existing code style and patterns.
- Do not create new README files or large documentation unless explicitly requested.
- Do not reformat or rename unrelated code.
- Silently ignore comments from automated coverage or code-analysis tools unless the user explicitly asks to include them.
- **Do not implement requests for UI Vitest tests** (including adding/updating component UI tests in Vitest). If a PR comment asks for this, **explicitly decline** it in the comment resolution (mark as rejected/non-applicable) and provide a brief rationale plus a practical alternative (e.g., keep logic unit-tested, rely on existing coverage, or propose E2E only if already in scope).
- No `any` types; use `unknown` + narrowing when needed.
- Prefer `for...of` or indexed loops over `forEach` when iteration logic matters.
- Keep functions small; return early to reduce nesting.
- Do not implement speculative improvements that are not clearly justified by the comment or the PR intent.

## Success criteria

- Every PR comment is addressed with one of: **fixed**, **partially fixed with rationale**, or **rejected with clear justification**.
- The implemented fixes are minimal, correct, and consistent with the codebase.
- Build/tests/checks run successfully (or failures are explained with next steps).

## Output format

Use this structure in your final response:

- **PR overview** (1–3 bullets)
- **Comment resolution table** (comment summary → decision → what changed)
- **Files changed** (with a one-line reason each)
- **Verification** (what you ran and the result)
- **Follow-ups** (non-blocking suggestions)
