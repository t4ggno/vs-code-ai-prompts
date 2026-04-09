---
description: Validate, stash, split a working branch into meaningful stacked commits and draft pull requests
agent: agent
---

# Split a Working Branch into Stacked Pull Requests

## Goal

Validate the current codebase, back up the current working tree in a stash, then split the work into a small, meaningful stack of commits and draft pull requests.

Use a stack prefix for every generated branch.
If the current branch is not the repository default branch, use the current branch name as that prefix.
If the current branch is the repository default branch and there are local changes to split, the agent should still create the stack branches, but it must first derive a concise kebab-case stack prefix from the actual change set or a user-provided task name. If it cannot infer a stable prefix with high confidence, it should ask the user for one instead of guessing badly. Never use the raw default branch name itself as the stack prefix.
Example source branch: `feature/cloud-1142-blabla`
Example generated branches: `feature/cloud-1142-blabla-base`, `feature/cloud-1142-blabla-auth`, `feature/cloud-1142-blabla-ui`

## Requirements

- Detect the current source branch and the repository default branch. Prefer the repository default branch; fall back to `main` only if necessary.
- If the current branch is not the default/base branch, treat the current branch name as the naming prefix for all generated stack branches.
- If the current branch is the default/base branch and there are local changes to split, do not stop for that reason alone. Instead, derive a stack prefix, create the generated stack branches from the default/base branch, and continue the workflow from those generated branches.
- Do not create or update a pull request from the default/base branch itself.
- This workflow is for splitting the current local working-tree changes into a PR stack. If there are no local changes to split, stop and report that there is nothing to do.
- Inspect the repository to detect the package manager, project scripts, PR template, and relevant quality gates before making any Git changes.
- Run the relevant quality gates first so the current work is validated before branching:
  - For TypeScript/JavaScript repos, check the changed code for redundant inline comments, reject `any` type usage, run ESLint, run type-checking, run tests if a project-defined test script is clearly relevant and stable, run build, and run the formatter if the project defines one.
  - For other stacks, run the closest repo-defined equivalent of lint, type-check, test, and build.
  - If a category is not defined by the repository, note that explicitly instead of guessing a command.
- If any required validation step fails, stop and report the failure with command output.
- Stash all local changes, including untracked files, using a named stash. Never use `git stash pop`, and do not drop the stash during this workflow.
- Build a split plan before creating branches:
  - Create one minimal `-base` branch for shared, common, or foundational changes that are required by multiple later slices.
  - Split the remaining files into meaningful, reviewable slices based on modules, scopes, domains, dependency layers, or feature boundaries.
  - Never create a PR with more than 20 changed files.
  - Treat 20 changed files as a hard ceiling, not a target. Prefer materially smaller PRs when practical, and split again if a slice is still overly large in diff text or changed lines even when it is under 20 files.
  - If a logical slice exceeds 20 files, split it again into smaller dependent slices.
  - Separate foundations, migrations, configuration, refactors, and feature-specific work when doing so keeps slices independently reviewable and valid.
  - Do not create random file buckets.
- Each branch must be reviewable and coherent on its own relative to its parent branch.
- Prefer a shallow stack rooted at `-base`:
  - The `-base` branch targets the repository default branch.
  - By default, every remaining slice branch should be created from and target the `-base` branch.
  - Only create a deeper dependent branch that targets another generated slice when this is necessary to preserve correctness, keep validation passing, or keep a PR under the hard size limits.
  - If you introduce a non-`-base` parent, document the dependency explicitly in the split plan and final report.
- For every generated branch:
  - Restore only the files assigned to that slice from the stash, without popping the stash.
  - Run the smallest relevant verification needed to prove that slice is valid relative to its parent branch.
  - If a slice cannot pass independently, move the missing shared files into an earlier branch or split the slice again. Do not ignore the failure.
  - Create a single-line Gitmoji commit message that starts with an uppercase letter and has no scope.
  - Push the branch safely without force-pushing.
  - Create a draft PR, or update the existing PR if one already exists for that branch.
- Before creating any branches, print the validation result and the split plan in a structured format.
- Every planned slice must either be executed or explicitly marked blocked/skipped with a reason. Never skip slices or files silently.
- Use `.github/pull_request_template.md` as the base for every PR body and keep its headings intact.
- Recompute every PR body from the actual diff between the branch and its intended base branch using the current merge base immediately before creating or updating that PR.
- Ensure every change-list item in every PR body starts with an uppercase letter.
- Ensure every PR title starts with a Gitmoji followed by a space and an uppercase letter. No scopes.
- Do not request reviewers, add labels, or post comments unless explicitly asked.
- Use only one-line commands so VS Code can auto-approve them.

## Constraints

- Do not pop, drop, or overwrite the backup stash.
- Do not create or update a PR when there is no meaningful diff for that slice.
- Do not split a tightly coupled change across multiple PRs if doing so would break linting, tests, type-checking, or build validation.
- Do not guess groupings, project commands, or PR content. Base them on the actual codebase, diff, dependency flow, and Git history.
- Do not exceed 20 changed files in any PR.
- Do not silently deepen the stack. Any generated branch that does not target `-base` must be explicitly justified.
- Do not amend, squash, rebase interactively, force-push, or bypass hooks unless explicitly instructed.
- Do not create duplicate PRs for the same generated branch.
- Do not leave files unassigned without explicitly reporting them and explaining why.
- Do not open a PR from the unsplit source branch itself. When the source branch is also the default/base branch, that means the agent must create the generated stack branches first and only open PRs from those generated branches.

## Reporting Contract

Before mutating Git history or creating branches, output:

1. `Validation summary`
   - detected package manager and scripts
   - checks run
   - pass/fail result per check
2. `Split plan`
   - a table with: branch name, intended base branch, scope/theme, file count, and why the slice exists
   - include the chosen stack prefix and whether it came from the current branch name or was derived because the workflow started on the default/base branch
3. `Risk notes`
   - any files that are ambiguous, tightly coupled, or likely to force a deeper dependency

After creating the stack, output:

1. `Stash backup`
   - stash reference and stash label
2. `Stack summary`
   - a table with: branch name, PR base, file count, commit message, verification run, and PR URL
3. `Blocked or skipped items`
   - every skipped slice or unassigned file with a reason

If a slice fails after the stash has been created:

- Stop creating new branches for the remaining slices.
- Keep the stash untouched.
- Preserve already-created branches and PRs.
- Report the failing slice, the failed verification, the affected files, and the recommended regrouping.

## Split Heuristics

Use these heuristics in order:

1. Put truly shared foundations in `-base`
   - Shared types, interfaces, schemas, utilities, primitives, config, migrations, infra glue, or abstractions used by 2 or more later slices.
2. Group by vertical or module boundary
   - Example: auth module, billing module, admin UI, API routes, shared design system, data layer.
3. Keep execution dependencies flowing downward
   - Example: schema or domain before service, service before API, API before UI.
4. Prefer a shallow stack rooted at `-base`
   - Keep later slices as siblings off `-base` unless a deeper dependency is required.
5. Prefer smaller, reviewer-friendly diffs
   - If two groups are only weakly related, separate them. If a slice is large in patch text or changed lines, split it again even when the file count is still acceptable.
6. Preserve working behavior
   - If one file is required to keep an earlier slice valid, move it earlier even if it is only tangentially related.
7. Cap every PR at 20 files
   - If needed, convert one large area into a mini-stack of its own.

## Detailed Step-by-Step Workflow

1. Detect repository context
   - Inspect the repository for the current branch, default branch, package manager, lockfiles, project scripts, build tooling, and `.github/pull_request_template.md`.
   - If the current branch is the default/base branch, derive a stack prefix before any PR branches are created.
   - Success criteria: the source branch, base branch, chosen stack prefix, validation commands, and PR template are known.

2. Validate the current work before branching
   - Run the relevant repo-defined quality gates in a sensible order: lint, type-check, tests when relevant, build, and formatter when defined.
   - For TypeScript or JavaScript repos, also scan the changed code for unnecessary inline comments and `any` type usage.
   - Success criteria: the working tree passes the required checks and is ready to split.

3. Inventory the current changes
   - Review staged, unstaged, and untracked files, plus the merge-base diff against the default branch when useful.
   - Produce a split plan that lists:
     - branch name
     - intended base branch
     - theme or scope
     - file count
     - why the files belong together
     - stack prefix source: current branch name or derived from the change set because the workflow started on the default/base branch
   - Output the split plan before creating branches so the execution path is visible and auditable.
   - Success criteria: every changed file is assigned to a meaningful slice or explicitly marked blocked.

4. Create a backup stash
   - Create a named stash that includes untracked files and record the exact stash reference.
   - Never pop or drop it during the workflow.
   - Success criteria: the working tree is clean and the backup stash reference is known.

5. Create the shared `-base` branch
   - Create a branch named `<stack-prefix>-base` from the repository default branch.
   - Restore only the shared or common files from the stash.
   - Run the smallest relevant verification for this slice.
   - Commit, push, and create or update a draft PR against the repository default branch.
   - Success criteria: the shared base changes live in a pushed branch with a draft PR.

6. Create the follow-up branches
   - For each remaining slice, create a branch named `<stack-prefix>-<scope>` from its chosen parent branch.
   - Default the parent branch to `<stack-prefix>-base`; only pick another generated branch when the split plan explicitly justifies that dependency.
   - Restore only that slice’s files from the stash.
   - Run the smallest relevant verification for this slice.
   - Commit, push, and create or update a draft PR against the chosen parent branch.
   - Success criteria: each slice is isolated, validated, committed, pushed, and has a draft PR.

7. Verify the stack
   - Confirm branch lineage, PR bases, file counts, and diffs are correct.
   - Make sure no PR exceeds 20 changed files, no files are duplicated across unrelated slices, and every non-`-base` parent was explicitly justified.
   - Success criteria: the stack is coherent, reviewable, and complete.

8. Report the result
   - Summarize the stash reference, created branches, parent relationships, commit messages, PR URLs, validation run for each slice, and any blocked, skipped, or unassigned files.
   - Success criteria: the user can review or continue the stack with full traceability.

## Success Criteria

- The codebase was validated before any branching work began.
- A named stash exists as a backup and was never popped.
- A minimal shared `-base` branch and draft PR were created first.
- The remaining work was split into meaningful stacked branches and draft PRs, with `-base` as the default parent for follow-up PRs.
- No generated PR contains more than 20 changed files.
- The prompt provides a structured validation summary, split plan, and final stack report.
- Every branch name uses the chosen stack prefix, which comes from the original source branch name unless the workflow started on the default/base branch and a prefix had to be derived first.
- Every commit and PR title uses a correct Gitmoji and uppercase one-line style with no scopes.
- Every PR body matches `.github/pull_request_template.md` and reflects only the real diff for that slice.
- The final report clearly lists the stack order, validation results, stash reference, and PR links.
