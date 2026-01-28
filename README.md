# VS Code AI Prompts

A curated set of VS Code Copilot prompt and instruction files used to guide automated code assistance in daily development workflows.

## What’s inside

- **Prompt templates** (`*.prompt.md`) for common workflows like fixes, improvements, pull requests, and security checks.
- **Instruction files** (`*.instructions.md`) that define coding standards and behavior rules.

## Usage

1. Place these files in your VS Code prompts directory.
2. Reference the prompts/instructions from your Copilot configuration or prompt picker.
3. Adjust or extend individual prompts to match your team’s workflow.

## Files (high level)

- `fix.prompt.md`, `improve-code.prompt.md`, `improve-ui.prompt.md` — focused improvement workflows
- `pull-request.prompt.md`, `check-pull-request-*.prompt.md` — PR review and validation
- `security-check-*.prompt.md` — security-focused review instructions
- `default.instructions.md`, `typescript*.instructions.md` — baseline and TypeScript rules

## Notes

These prompts are designed to be concise, enforce consistent style, and reduce back-and-forth during code review and maintenance.
