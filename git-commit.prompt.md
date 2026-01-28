---
description: Commit and push changes safely
agent: agent
---

Goal: stage changes, create a single-line commit message, and push safely.

## Requirements

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

1. Stage all files
   - Command: `git add -A`
   - Success criteria: all relevant changes are staged.

2. Check changed files
   - Command: `git status -sb` and `git diff --name-only --cached`
   - Success criteria: staged file list is visible and correct.

3. Review staged changes for commit message
   - Command: `git diff --cached`
   - Success criteria: staged diff content is visible to determine the correct Gitmoji and message.

4. Commit with proper message (one line)
   - Template: `<Gitmoji Icon> <Uppercase message>`
   - Example: `✨ Add transport reason to relocation order`
   - Success criteria: commit created with a single-line message and correct Gitmoji.

5. Push changes (local AND remote)
   - Command: `git push` (if upstream not set, use `git push -u origin <branch>`)
   - Success criteria: push succeeds and remote branch is updated.

## Success Criteria

- All commands completed successfully in order.
- Files staged and verified.
- Commit created with correct Gitmoji and uppercase message.
- Changes pushed to remote.
