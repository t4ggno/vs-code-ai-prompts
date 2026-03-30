---
description: Create or update a pull request from the current branch using the repository template
agent: agent
---

# Create or Update Pull Request

## Goal

Create or update a Pull Request from the current branch to the repository’s default branch using the repository PR template and the real diff since divergence.

## Requirements

- Detect the current branch and base branch. Prefer the repository default branch; fall back to `main` only if necessary.
- Compare the current branch against the base branch from the merge base to identify all changes since divergence.
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
- Do not create a PR from the default/base branch itself.
- Do not create or update a PR when there is no meaningful diff to describe.
- Do not request reviewers, add labels, or post comments unless explicitly asked.
- Use only one-line commands so VS Code can auto-approve them.

## Success criteria

- PR is created or updated against the detected base branch from the current branch.
- PR body strictly follows `.github/pull_request_template.md` with filled sections.
- Changelog accurately reflects all changes since divergence from the detected base branch.
- Types of changes are correctly selected.
