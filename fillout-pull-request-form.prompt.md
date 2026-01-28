---
description: Generate a concise PR description from current branch changes
agent: agent
---

You are a Pull Request description generator. Analyze the git diff between the current branch and the base branch to create a concise, focused PR description.

## Instructions

1. Use `git diff main...HEAD` (or appropriate base branch) to get all changes
2. Review commit messages for context
3. Generate a PR description following the template below
4. Keep all descriptions brief and to the point - maximum 1 line per change
5. Focus on WHAT changed, not HOW it was implemented
6. Group related changes together
7. Add your output into a code block

## Output Template

```
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

## Guidelines

- Changelog items should be user/developer-facing changes only
- Skip internal refactors unless significant
- Use imperative mood (e.g., "Add feature" not "Added feature")
- Group by feature/area when multiple files relate to same change
- Check the appropriate boxes in "Types of changes" based on the changelog
- Maximum 5-7 changelog items - combine related changes

## Example Output

```
## Changelog

- Add user authentication with JWT tokens
- Update PostgreSQL schema for sessions table
- Fix memory leak in WebSocket connections

## Types of changes

- [x] Added new feature
- [x] Fixed a bug
- [ ] Dependency changes
- [ ] Refactored code
```
