---
description: Generate a concise, ready-to-paste PR description from the current branch changes
agent: agent
---

# Generate Pull Request Description

Analyze the git diff between the current branch and the base branch to create a concise, focused PR description that reviewers can trust.

## Instructions

1. Detect the correct base branch (prefer the repository default branch; otherwise use the branch configured for the current work).
2. Use the diff from the merge base to the current branch so the description covers the full PR scope.
3. Review commit messages for context, but trust the diff over commit wording if they conflict.
4. If `.github/pull_request_template.md` exists and is relevant, match its headings; otherwise use the fenced template at the end of this file.
5. Keep descriptions brief and to the point: maximum 1 line per change.
6. Focus on **what changed and why it matters**, not implementation trivia.
7. Group related changes together and collapse minor noise.
8. Return the ready-to-paste PR body directly, without wrapping it in a code block unless the user explicitly asks.

## Guidelines

- Changelog items should be user/developer-facing changes only
- Skip internal refactors unless significant
- Use imperative mood (e.g., "Add feature" not "Added feature")
- Group by feature/area when multiple files relate to same change
- Check the appropriate boxes in "Types of changes" based on the changelog
- Maximum 5-7 changelog items - combine related changes
- Do not guess missing context; if something important cannot be inferred from the diff, leave it out.
- Do not mention file names unless they materially help the reviewer understand the change.

## Success criteria

- The description matches the actual PR diff.
- The wording is concise, reviewer-friendly, and free of guesswork.
- The result is ready to paste into the PR body immediately.

## Output Template

Use the following markdown block as the complete template content when no repository PR template overrides it.

```markdown
## Changelog

- [Brief description of change 1]
- [Brief description of change 2]
- [Brief description of change 3]

## Types of changes

- [ ] Added new feature
- [ ] Fixed a bug
- [ ] Dependency changes
- [ ] Refactored code
```
