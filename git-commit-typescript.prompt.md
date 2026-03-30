---
description: Run TypeScript quality gates, then commit and push safely
agent: agent
---

# TypeScript Commit and Push

## Goal

Run the relevant TypeScript quality gates in order, then stage, commit, and push the changes safely.

## Requirements

- Check for redundant inline comments to ensure code is cleaner (do not remove them automatically; fail if found). JSDoc comments/documentation comments are allowed.
- Ensure that `any` is NOT used as a type. Fail if detected.
- Detect the project’s package manager and quality scripts from the workspace before running commands.
- Run ESLint and ensure it completes successfully.
- Run TypeScript type-checking and ensure it completes successfully.
- Run the formatter if the project defines one.
- Stage all modified files.
- List changed files after staging.
- If there are no staged changes after staging, stop and report that there is nothing to commit.
- Create a single-line commit message that:
  - Starts with an uppercase letter.
  - Includes the correct Gitmoji for the change type (see table below).
  - Uses only one line (no body).
  - Directly starts with the description (NO scopes like `(feat)` allowed).
- Push changes to the remote tracking branch (ensure upstream is set if missing).

## Constraints

- Follow the exact order of steps below.
- Do not skip any step.
- Use only one Gitmoji shortcode from the list.
- If any step fails, stop and report the failure with the command output.
- Do not amend, squash, force-push, or bypass hooks unless explicitly instructed.
- Do not guess commands. Prefer project-defined scripts or clearly supported fallbacks.

## Gitmoji Selection

Pick exactly one icon based on the primary change:

- ✨ Introduce a new feature
- 🐛 Fix a bug
- 📝 Add or update documentation
- ♻️ Refactor code (no functional change intended)
- 🚀 Deploy stuff / release to production
- ✅ Add, update, or pass tests
- 🔥 Remove code or files
- 🎨 Improve structure/format of the code (style/cleanup)
- 🔧 Add or update configuration files
- 🚑️ Critical hotfix

## Detailed Step-by-Step Workflow

1. Detect project commands
   - Action: Inspect the workspace for `package.json`, lockfiles, `tsconfig.json`, and relevant scripts.
   - Success criteria: the correct package manager and the lint/type-check/format commands are known.

2. Check for unnecessary comments
   - Action: Scan the code changes for inline comments that state the obvious (e.g. `// assigns value`). JSDoc comments (starting with `/**`) are allowed and encouraged. Do not remove unnecessary comments automatically. If found, stop and report the failure.
   - Success criteria: Code is readable without "what-it-does" inline comments.

3. Check for `any` type usage
   - Action: Scan the code changes for usages of the `any` type. If found, stop and report the failure.
   - Success criteria: No `any` type usage found.

4. Run ESLint
   - Command: use the project’s lint script or documented equivalent.
   - Success criteria: command exits with code 0 and no lint errors remain.

5. Run TypeScript type-check
   - Command: use the project’s type-check script; if none exists but `tsconfig.json` is present, use `tsc --noEmit` through the detected package manager or toolchain.
   - Success criteria: command exits with code 0 and no type errors remain.

6. Run formatter
   - Command: use the project’s formatting script if one exists.
   - Success criteria: formatting is applied successfully, or the absence of a formatter is explicitly noted without guessing a command.

7. Stage all files
   - Command: `git add -A`
   - Success criteria: all relevant changes are staged.

8. Check changed files
   - Command: `git status -sb` and `git diff --name-only --cached`
   - Success criteria: staged file list is visible and correct. If the staged diff is empty, stop and report that there is nothing to commit.

9. Review staged changes for commit message
   - Command: `git diff --cached`
   - Success criteria: staged diff content is visible to determine the correct Gitmoji and message.

10. Commit with proper message (one line)
   - Template: `<Gitmoji Icon> <Uppercase message>`
   - Example: `✨ Add transport reason to relocation order`
   - Success criteria: commit created with a single-line message and correct Gitmoji.

11. Push changes to the remote branch
    - Command: `git push` (if upstream not set, use `git push -u origin <branch>`)
    - Success criteria: push succeeds and remote branch is updated.

## Success Criteria

- All commands completed successfully in order.
- No unnecessary inline comments found.
- No `any` type usage found.
- No linting or type-checking errors.
- Formatting applied when the project defines a formatter.
- Files staged and verified.
- Commit created with correct Gitmoji and uppercase message.
- Changes pushed to remote.
