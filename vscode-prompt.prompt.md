---
description: Create a new Copilot prompt file or improve an existing one using current best practices and local prompt-library conventions
agent: agent
argument-hint: describe the prompt to create, or name an existing .prompt.md file to improve
---

# VS Code: Create or Improve Prompt Files

## Goal

Create a new `.prompt.md` file or improve an existing one so it is clear, repeatable, and less likely to produce ambiguous or error-prone results.

Use local prompt-library conventions from [README.md](./README.md), [default.instructions.md](./default.instructions.md), and neighboring `*.prompt.md` files as the primary template. Use current official GitHub Copilot and VS Code documentation as the best-practice baseline when refining the prompt.

## Inputs

- The user's natural-language request
- An optional target prompt file name or path
- The current selection or active file if it is relevant
- Existing prompt files in this folder
- Current online guidance for Copilot prompt files and reusable prompt design

Treat the user's plain-language request as sufficient input. Do not require `file=`, `mode=`, or other parameter syntax when the intent is already clear.

## Scenario selection

Choose exactly one primary scenario.

### Scenario 1 — Create a new prompt file

Use this scenario when the user asks for a new prompt, describes a new reusable workflow, or when no existing prompt file is a good match for the request.

### Scenario 2 — Improve an existing prompt file

Use this scenario when:

- the user names an existing `.prompt.md` file,
- the active file is an existing prompt file, or
- the user's request clearly matches one existing prompt better than the others.

If the user names a file but gives little or no extra detail, infer the improvement goals from the file's current purpose, the local prompt-library conventions, and current Copilot best practices.

If no specific file is named, inspect the prompt library at a high level, identify the single best matching existing prompt, and improve only that file. If no reasonable match exists, switch to **Scenario 1** instead of forcing an unrelated edit.

## Required workflow

1. **Search online first for current Copilot prompt best practices.**
	- Start with official VS Code and GitHub Copilot documentation.
	- Prefer verified guidance about prompt-file frontmatter, structure, scope, input handling, output formatting, and clarity.
	- Use additional non-official sources only when they add something materially useful.
	- If sources conflict, prefer official documentation and the local repository conventions.

2. **Inspect the local prompt library before writing.**
	- Read [README.md](./README.md).
	- Review the target prompt if one exists.
	- Review at least 2 relevant neighboring prompt files as templates.
	- Match the library's tone and section structure where practical.

3. **Determine the target and the scenario.**
	- For **Scenario 1**, infer the prompt purpose, scope, and likely file name from the user's request.
	- For **Scenario 2**, identify the prompt file to improve.
	- If no file is named, explain which existing prompt was selected and why.
	- Ask a focused clarifying question only if a missing detail would materially change the prompt's purpose or file selection.

4. **Draft or improve the prompt to make it deterministic.**
	- Use valid YAML frontmatter.
	- Include a concise `description`.
	- Use `agent: agent` unless a different agent is clearly justified.
	- Add `argument-hint` when it helps users invoke the prompt correctly.
	- Use clear sections such as **Goal**, **Inputs**, **Required workflow**, **Constraints**, **Output**, and **Success criteria** when they improve reliability.
	- Preserve the existing slash-command name unless the user explicitly asks to rename the file.

5. **Reduce ambiguity and failure modes.**
	- Remove vague wording, overlapping instructions, and contradictory requirements.
	- Prefer explicit decision rules over soft language such as “use your judgment” when a repeatable rule is possible.
	- Tell the AI what to do when input is missing, ambiguous, or out of scope.
	- Keep the default scope narrow and explicit.
	- Separate confirmed facts from assumptions when the prompt asks for analysis, review, or reporting.

6. **Keep the change tightly scoped.**
	- Create or edit only the relevant prompt file unless the user explicitly asks for broader prompt-library changes.
	- Do not rewrite unrelated prompts for style consistency alone.
	- If the target file is empty or only contains placeholder text, replace it with a complete prompt.

7. **Validate the result.**
	- Ensure the file name is kebab-case and ends with `.prompt.md`.
	- Ensure YAML frontmatter is valid and only uses supported metadata.
	- Ensure the body is internally consistent and easy to follow.
	- Ensure relative links, if any, are correct.
	- Ensure the final prompt matches local conventions and the user's intent.

## Prompt-writing standards

Apply these standards when authoring or improving the prompt:

- Prefer concise, imperative instructions.
- Keep one responsibility per section.
- Make scope rules explicit.
- Prefer stable output formats that are easy to review.
- Reuse local conventions before inventing a new prompt style.
- Avoid unsupported metadata fields, made-up tools, or speculative behavior.
- Include small examples only when they materially reduce ambiguity.

## File naming rules

- Use kebab-case.
- End the file name with `.prompt.md`.
- Choose a name that describes the user task.
- If improving an existing prompt, keep the current file name unless the user explicitly asks to rename it.

## Output

Report the result in this order:

- **Scenario chosen**
- **Best-practice references used**
- **Target file**
- **What changed**
- **Why this prompt is more reliable**
- **Assumptions or follow-ups**

## Success criteria

- The prompt matches the user's intended task.
- The workflow is clear, repeatable, and resistant to ambiguous execution.
- The file follows local prompt-library conventions.
- The prompt uses current Copilot prompt-file best practices without unnecessary complexity.
