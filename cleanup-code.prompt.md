---
description: Clean up a defined scope or the full workspace by removing dead code, unused files, and other safe clutter
agent: agent
argument-hint: optional scope=path/to/file-or-folder
---

# Cleanup: Code

## Goal

Clean up a clearly defined scope, or the full workspace when no scope is provided, by removing dead code, unused files, obsolete references, and other safe clutter without changing intended behavior.

## Scope rules

- Clean up only the scope the user names.
- If the user provides `scope=...`, limit the cleanup pass to that file or folder.
- If the user does not provide a scope, treat the entire workspace as the scope.
- Do not silently narrow an unscoped cleanup request to the active file, current selection, or attached files.
- In full-workspace mode, first create a file-by-file plan that accounts for every file in the workspace.
- In full-workspace mode, files may be marked as **cleaned**, **removed**, **no changes needed**, **skipped** (with reason), or **blocked** (with reason), but no file should be left unaccounted for.
- Generated files, dependencies, binaries, build artifacts, lockfiles, and other files that should not be manually cleaned may be skipped, but they must still be listed in the plan with a short reason unless the user explicitly included them.
- Never expand scope beyond what is necessary to keep the target working after cleanup.

## Required workflow

1. Determine the actual scope first and state whether you are running in focused mode or full-workspace mode.
2. Inspect the target code, its direct dependencies, and nearby patterns before planning deletions or removals.
3. In full-workspace mode, enumerate the workspace files and create a short per-file checklist before editing.
4. Identify concrete cleanup candidates such as dead code, unused files, stale exports, unreachable branches, temporary debugging code, redundant imports, obsolete docs, or leftover scaffolding.
5. Verify that each cleanup candidate is truly unused before removing it. Check imports, references, configuration files, build/test scripts, documentation links, and any other obvious usage paths.
6. Make a short per-file cleanup plan before editing.
7. Clean up in small, behavior-preserving steps.
8. When deleting a file, also remove now-dead references to it in imports, indexes, configs, and documentation that are within scope.
9. After each file, update the checklist to mark it as cleaned, removed, no changes needed, skipped, or blocked, then continue until every file in scope has been accounted for.
10. Verify the touched area with the most relevant checks available.

## Cleanup priorities

- Unused files that can be safely removed
- Dead code, unreachable branches, and stale helpers
- Temporary debugging code, commented-out code, and leftover scaffolding
- Redundant imports, exports, styles, or configuration entries made obsolete by removals
- Obsolete documentation references caused by cleanup work

## Constraints

- Preserve current behavior unless the user explicitly asks for functional changes.
- Do not turn cleanup into a broad refactor or redesign.
- Do not rewrite healthy code just to make it look different.
- Do not remove files or code when usage is uncertain; mark them as blocked with the reason instead.
- Do not add comments instead of performing the cleanup itself.
- Do not delete generated or external files unless the user explicitly includes them.

## Output

Use this structure:

- **Cleanup scope**
- **File-by-file plan**
- **Cleanup actions taken**
- **Files removed**
- **Files changed**
- **Verification**
- **Follow-ups or blocked items**

## Success criteria

- Only cleanup-related changes were made.
- Behavior is preserved.
- If full-workspace mode was used, every file in the workspace was accounted for and marked as cleaned, removed, no changes needed, skipped, or blocked before stopping.
- Any deleted files were verified as unused and their local references were cleaned up.
