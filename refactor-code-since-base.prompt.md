---
description: Refactor only the code changed since the current branch diverged from its base branch
agent: agent
argument-hint: optional scope=path/to/file-or-folder base=origin/main
---

# Refactor: Code Since Base

## Goal

Improve code quality only within the Git diff that represents the current branch's changes since it diverged from the base branch, without changing intended behavior.

## Example invocations

- `/refactor-code-since-base`
- `/refactor-code-since-base scope=src/features/auth`
- `/refactor-code-since-base base=origin/release/1.4 scope=packages/web`

## Diff scope rules

- Default scope is the current branch delta, not the whole workspace, active file, or selection.
- Detect the base ref in this order:
  1. active PR base branch, if available
  2. repository default branch's remote-tracking ref (for example `origin/main`)
  3. local default branch (for example `main` or `master`)
- An explicit user-supplied base ref overrides the auto-detected base.
- If the effective base ref cannot be resolved confidently, ask one focused clarifying question instead of guessing.
- If the user supplies `scope=...`, intersect that scope with the Git diff. Do not refactor files outside that intersection.
- If there is no meaningful diff relative to the resolved base, explain that and stop.

## Required Git analysis

1. Determine the current branch and the effective base ref.
2. Use merge-base semantics so the diff includes both committed branch changes and current working-tree changes.
3. Prefer a diff based on the merge base with the resolved base ref, for example:
   - changed files: `git diff --name-only --diff-filter=ACMR --merge-base <base> -- [scope]`
   - exact hunks: `git diff -U0 --diff-filter=ACMR --merge-base <base> -- [scope]`
4. Treat added files as fully in scope.
5. Treat deleted files as review-only; do not try to refactor deleted paths.
6. If the base branch was clearly rewritten and a normal merge-base would over-include unrelated commits, consider `git merge-base --fork-point <base> HEAD` only when reflog evidence supports it. Otherwise stick to the normal merge-base behavior.
7. Use source control changes as the primary scope signal and use diagnostics, terminal output, or test failures only as supporting context for the touched diff.

## Required workflow

1. Inspect the diff first and list the changed files or hunks you plan to touch.
2. Read the surrounding code, direct dependencies, and nearby patterns for each changed hunk before editing.
3. Make a short per-file plan focused on the changed hunks.
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

- Do not refactor untouched files just because they are nearby.
- Do not rewrite whole files when only a few hunks changed, unless the entire file is newly added.
- Do not change behavior unless the user explicitly asks for it.
- Do not add comments instead of improving the code itself.
- Do not optimize based on guesswork.
- Do not rely only on staged or unstaged diffs if that would miss committed branch changes; use the merge-base diff.

## Edge cases

- If the current branch is detached, explain the resolved refs you are using before proceeding.
- If the repository has no suitable base ref locally, ask for one or tell the user to fetch the relevant remote branch.
- If the diff is too large for one safe refactor pass, propose a smaller scoped follow-up instead of broad cleanup.

## Output

Use this structure:

- **Base diff**
- **Refactor scope**
- **Changed hunks addressed**
- **Files changed**
- **Verification**
- **Follow-ups / skipped hunks**

## Success criteria

- Only the branch diff, or the intersection of the branch diff and the user scope, was refactored.
- Exact changed line ranges were considered before editing.
- Behavior is preserved.
- The result is easier to read and maintain without unrelated cleanup.
