---
agent: agent
---
Goal: run linting, type-checking, formatting, and Git operations in order, then commit and push the changes safely.

## Requirements
- Check for redundant comments to ensure code is cleaner (do not remove them automatically; fail if found).
- Ensure that `any` is NOT used as a type. Fail if detected.
- Run ESLint and ensure it completes successfully.
- Run TypeScript type-checking with `tsc` and ensure it completes successfully.
- Run the formatter and ensure it completes successfully.
- Stage all modified files.
- List changed files after staging.
- Create a single-line commit message that:
  - Starts with an uppercase letter.
  - Includes the correct Gitmoji for the change type (see table below).
  - Uses only one line (no body).
  - Directly starts with the description (NO scopes like `(feat)` allowed).
- Push changes to both local and remote tracking branches (ensure upstream is set if missing).

## Constraints
- Follow the exact order of steps below.
- Do not skip any step, even if previous commands report no changes.
- Use only one Gitmoji shortcode from the list.
- If any step fails, stop and report the failure with the command output.
- Do not amend or squash commits unless explicitly instructed.

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
1) Check for unnecessary comments
	- Action: Scan the code changes for comments that state the obvious. Do not remove them automatically (that is the user's task). If found, stop and report the failure.
	- Success criteria: Code is readable without "what-it-does" comments.

2) Check for `any` type usage
	- Action: Scan the code changes for usages of the `any` type. If found, stop and report the failure.
	- Success criteria: No `any` type usage found.

3) Run ESLint
	- Command: `npm run lint` (or the project’s documented ESLint command).
	- Success criteria: command exits with code 0 and no lint errors remain.

4) Run TypeScript type-check
	- Command: `npx tsc --noEmit` (or the project’s documented type-check command).
	- Success criteria: command exits with code 0 and no type errors remain.

5) Run formatter
	- Command: `npm run format` (or the project’s documented formatting command).
	- Success criteria: command exits with code 0 and formatting is applied.

6) Stage all files
	- Command: `git add -A`
	- Success criteria: all relevant changes are staged.

7) Check changed files
	- Command: `git status -sb` and `git diff --name-only --cached`
	- Success criteria: staged file list is visible and correct.

8) Review staged changes for commit message
	- Command: `git diff --cached`
	- Success criteria: staged diff content is visible to determine the correct Gitmoji and message.

9) Commit with proper message (one line)
	- Template: `<Gitmoji Icon> <Uppercase message>`
	- Example: `✨ Add transport reason to relocation order`
	- Success criteria: commit created with a single-line message and correct Gitmoji.

10) Push changes (local AND remote)
	- Command: `git push` (if upstream not set, use `git push -u origin <branch>`)
	- Success criteria: push succeeds and remote branch is updated.

## Success Criteria
- All commands completed successfully in order.
- No unnecessary comments found.
- No `any` type usage found.
- No linting or type-checking errors.
- Formatting applied.
- Files staged and verified.
- Commit created with correct Gitmoji and uppercase message.
- Changes pushed to remote.
