# VS Code AI Prompts

A curated prompt library for GitHub Copilot in VS Code. This repository contains reusable `*.prompt.md` and `*.instructions.md` files for common engineering workflows such as debugging, refactoring, documentation, testing, security reviews, and pull request preparation.

The goal is simple: make prompt-driven work more consistent, more evidence-based, and less dependent on repeating the same setup instructions in every chat.

## What this repository contains

- **Task prompts** for focused workflows such as fixing code, reviewing pull requests, generating tests, or improving a UI.
- **Shared instruction files** that define baseline coding behavior and TypeScript-specific expectations.
- **Prompt templates with structure**: the prompt files typically include a clear goal, expected inputs, required workflow steps, constraints, output expectations, and success criteria.
- **Scope-aware guidance**: many prompts are designed to stay within the current file, selection, view, or explicitly provided target instead of encouraging broad changes.

## Prompt categories

### Debugging and repair

- `debug-analyze-error.prompt.md` — Diagnose an error, identify the root cause, and implement or recommend the smallest safe fix.
- `fix-code-general.prompt.md` — Stabilize a workspace until it reaches a clean validation state.
- `fix-nestjs-controller.prompt.md` — Fix a NestJS controller issue with the smallest safe change.

### Refactoring, design, and explanation

- `design-scaffold-feature.prompt.md` — Design a feature and scaffold the minimal file structure needed to implement it.
- `explain-complex-logic.prompt.md` — Explain complex code in a grounded, easy-to-follow way.
- `guide-web-development.prompt.md` — General web-development implementation prompt with evidence-based workflows.
- `refactor-code.prompt.md` — Refactor a defined scope for clarity, maintainability, and safe performance wins.
- `refactor-component.prompt.md` — Improve a component with clearer structure, accurate docs, and logic-focused tests.
- `refactor-user-interface.prompt.md` — Improve a UI surface with clear, codebase-aligned UX refinements.

### Documentation

- `docs-generate-jsdoc.prompt.md` — Add accurate JSDoc or TSDoc comments to existing code without changing behavior.
- `docs-generate-readme.prompt.md` — Generate or update README content grounded in the actual code.

### Git and pull request workflows

- `git-check-pull-request-comments.prompt.md` — Validate PR review comments, apply justified changes, and prepare clear resolution notes.
- `git-check-pull-request-implementation.prompt.md` — Review PR implementation for correctness, completeness, and risk without guessing requirements.
- `git-commit.prompt.md` — Stage relevant changes, create a clean Gitmoji commit, and push safely.
- `git-commit-typescript.prompt.md` — Run TypeScript quality gates, then commit and push safely.
- `git-create-pull-request.prompt.md` — Create or update a pull request from the current branch using the repository template.
- `git-write-pull-request-description.prompt.md` — Generate a concise, ready-to-paste PR description from the current branch changes.

### Security review

- `security-check-general.prompt.md` — Perform a comprehensive general security review.
- `security-check-nestjs.prompt.md` — Perform a comprehensive NestJS security review.
- `security-check-nextjs.prompt.md` — Perform a comprehensive Next.js security review.
- `security-check-react.prompt.md` — Perform a comprehensive React app security review.

### Testing

- `test-generate-e2e.prompt.md` — Generate stable end-to-end tests for a real user workflow.
- `test-generate-unit.prompt.md` — Generate focused unit tests for a defined source file or symbol.

## Instruction files

- `default.instructions.md` — Baseline working style and editing rules such as investigating first, staying in scope, making the smallest safe change, and verifying results.
- `typescript.instructions.md` — TypeScript-specific guidance for strict typing, project-aware validation, React/Next.js/NestJS conventions, and code organization.
- `typescript-cheatsheet.instructions.md` — A modern TypeScript reference with preferred patterns, type-system examples, and practical guidance.

## Prompt design conventions

Across the repository, the prompt files follow a few common principles:

- **Inspect before acting** — prompts favor reading the codebase and understanding the target before making changes.
- **Stay in scope** — refactor and UI prompts generally default to the current selection, component, or active view unless the user asks for a broader pass.
- **Use explicit constraints** — prompts call out what not to do, such as inventing behavior, changing unrelated files, or making speculative claims.
- **Prefer grounded output** — review and security prompts emphasize confirmed findings, evidence, and clearly separated assumptions or open questions.
- **Keep workflows repeatable** — many prompts define required steps and expected output formats so results are easier to review and reuse.

## Usage

1. Copy the files into your VS Code prompts directory:

	- Windows: `%appdata%\Code\User\prompts`
	- Windows (Insiders): `%appdata%\Code - Insiders\User\prompts`

2. Open the prompt picker or reference the files from your Copilot configuration.
3. Choose the prompt that matches the task and provide a target scope when needed, such as a file, symbol, current selection, or view.
4. Combine the prompts with the instruction files when you want stronger baseline rules for coding style and behavior.
5. Adjust individual prompt wording to match your team’s conventions, approval process, or stack-specific needs.

## When this repo is useful

This repository is a good fit if you want to:

- standardize how routine Copilot-assisted tasks are framed,
- reduce back-and-forth for common workflows,
- keep prompts focused on small, verifiable changes,
- maintain separate prompts for general, TypeScript, React, Next.js, and NestJS scenarios,
- build a prompt library that teammates can reuse and extend.

## Notes

These prompts are intentionally practical rather than generic. They are written to encourage grounded analysis, minimal safe changes, and outputs that are easy to validate in real development workflows. In short: less vague AI magic, more useful engineering muscle.
