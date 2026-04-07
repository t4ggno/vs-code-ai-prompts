---
description: Refactor only files with current uncommitted changes, excluding already committed branch changes
agent: agent
argument-hint: optional scope=path/to/file-or-folder
---

# Refactor: Code Uncommitted

## Goal

Improve code quality only within the current uncommitted changes in the working tree and index, without changing intended behavior and without reviewing files that were only changed by earlier committed work on the branch.

## Example invocations

- `/refactor-code-uncommitted`
- `/refactor-code-uncommitted scope=src/components`
- `/refactor-code-uncommitted scope=packages/api/src`

## Diff scope rules

- Default scope is the current uncommitted delta relative to `HEAD`, not the full branch delta.
- Do not include previously committed files on the current branch unless they also have current uncommitted changes.
- If the user supplies `scope=...`, intersect that scope with the current uncommitted diff. Do not refactor files outside that intersection.
- Include staged changes, unstaged changes, and untracked files.
- If there is no meaningful uncommitted diff, explain that and stop.

## Required Git analysis

1. Determine the current branch and `HEAD`.
2. Build the scope from the current uncommitted state only.
3. Prefer commands like these:
   - tracked changed files: `git diff --name-only --diff-filter=ACMR HEAD -- [scope]`
   - exact tracked hunks: `git diff -U0 --diff-filter=ACMR HEAD -- [scope]`
   - untracked files: `git ls-files --others --exclude-standard -- [scope]`
4. Treat staged or unstaged modifications as in scope.
5. Treat untracked files as fully in scope because they have no diff hunks relative to `HEAD` yet.
6. Treat deleted files as review-only; do not try to refactor deleted paths.
7. If the user explicitly wants only staged or only unstaged changes, narrow the Git commands accordingly. Otherwise consider both.
8. If `HEAD` does not exist yet, fall back to staged files plus untracked files instead of trying to diff against `HEAD`.
9. Use source control changes as the primary scope signal and use diagnostics, terminal output, or test failures only as supporting context for the touched diff.

## Required workflow

1. Inspect the current uncommitted diff first and list the changed files or hunks you plan to touch.
2. Read the surrounding code, direct dependencies, and nearby patterns for each changed hunk before editing.
3. Make a short per-file plan focused on the current uncommitted hunks.
4. Refactor in small, behavior-preserving steps.
5. Keep edits inside the changed hunks when possible.
6. Expand slightly outside a changed hunk only when required to keep the touched code correct, compilable, or readable. Explicitly justify each such expansion.
7. Prefer existing helpers, components, and abstractions over creating new ones.
8. Verify the touched area with the most relevant checks available.

## Refactoring priorities

- Clear names and responsibilities inside touched diff areas
- Smaller functions and earlier exits
- Reduced nesting and duplication local to the changed code
- Safer typing and narrower contracts
- Simple performance wins that remove repeated work in the changed paths
- Modern TypeScript features when they improve correctness or readability

## Constraints

- Do not inspect or refactor files that are only part of previously committed branch history.
- Do not rewrite whole files when only a few hunks changed, unless the entire file is untracked or newly added.
- Do not change behavior unless the user explicitly asks for it.
- Do not add comments instead of improving the code itself.
- Do not optimize based on guesswork.
- Do not switch to merge-base diff logic for this prompt; this prompt is intentionally limited to the current uncommitted state.

## Edge cases

- If the repository is in an unborn state, state that assumption clearly and limit scope to staged and untracked files.
- If only untracked files exist, treat those files as the entire scope and do not attempt hunk-based filtering.
- If the uncommitted diff is too large for one safe pass, propose a smaller scoped follow-up instead of broad cleanup.

## Output

Use this structure:

- **Uncommitted diff**
- **Refactor scope**
- **Changed hunks addressed**
- **Files changed**
- **Verification**
- **Follow-ups / skipped hunks**

## Success criteria

- Only the current uncommitted diff, or the intersection of that diff and the user scope, was refactored.
- Previously committed branch-only changes were ignored unless they also had current uncommitted edits.
- Exact changed line ranges were considered before editing.
- Behavior is preserved.
- The result is easier to read and maintain without unrelated cleanup.
