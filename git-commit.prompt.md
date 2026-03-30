---
description: Stage relevant changes, create a clean Gitmoji commit, and push safely
agent: agent
---

# Commit and Push Safely

## Goal

Stage the intended changes, create a single-line Gitmoji commit message, and push the commit to the tracked remote branch without using unsafe shortcuts.

## Requirements

- Confirm the repository status and current branch before committing.
- Stage all modified files.
- List changed files after staging.
- If there are no changes after staging, stop and report that there is nothing to commit.
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

1. Confirm repository state
   - Check current branch and working tree status.
   - Success criteria: the repo and target branch are known.

2. Stage all files
   - Command: `git add -A`
   - Success criteria: all relevant changes are staged.

3. Check changed files
   - Command: `git status -sb` and `git diff --name-only --cached`
   - Success criteria: staged file list is visible and correct. If the staged diff is empty, stop and report that there is nothing to commit.

4. Review staged changes for commit message
   - Command: `git diff --cached`
   - Success criteria: staged diff content is visible to determine the correct Gitmoji and message.

5. Commit with proper message (one line)
   - Template: `<Gitmoji Icon> <Uppercase message>`
   - Example: `✨ Add transport reason to relocation order`
   - Success criteria: commit created with a single-line message and correct Gitmoji.

6. Push changes to the remote branch
   - Command: `git push` (if upstream not set, use `git push -u origin <branch>`)
   - Success criteria: push succeeds and remote branch is updated.

## Success Criteria

- All commands completed successfully in order.
- Files staged and verified.
- Commit created with correct Gitmoji and uppercase message.
- Changes pushed to remote.
