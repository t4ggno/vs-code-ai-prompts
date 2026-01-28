---
description: Create or update a PR using the repository template
agent: agent
---

Goal: Create or update a Pull Request based on changes since the branch diverged from `main`, using the repository template.

## Requirements

- Compare current branch against `main` to identify all changes since divergence. Use git history/diff to summarize the full set of changes included in this PR.
- Ensure every element in the change list starts with an uppercase letter.
- Ensure the PR title starts with a gitmoji (matching the commits) followed by a space and an uppercase letter. NO scopes (e.g. `(feat)`) allowed.
- Use the template from `.github/pull_request_template.md` as the base and keep its headings intact.
- If a PR already exists for the current branch, update its body to match the template and reflect the current diff.
- If no PR exists for the current branch, create a new PR in **draft** mode.
- If updating a PR that is already a draft, keep it in draft mode; do not change draft status.

## Constraints

- Do not modify the template structure or headings.
- Do not add additional sections beyond the template.
- Keep the PR title concise and aligned with the main change set.
- Only include information backed by the actual diff and git history; no guesses.
- Use only one-line commands so VS Code can auto-approve them.

## Success criteria

- PR is created or updated against `main` from the current branch.
- PR body strictly follows `.github/pull_request_template.md` with filled sections.
- Changelog accurately reflects all changes since divergence from `main`.
- Types of changes are correctly selected.
