---
description: Create a new Copilot Agent Skill or improve existing SKILL.md files using current best practices and local skill templates
agent: agent
argument-hint: describe the skill to create, or name an existing skill folder / SKILL.md file to improve
---

# VS Code: Create or Improve Skills

## Goal

Create a new Copilot Agent Skill or improve existing `SKILL.md` files so they are clear, reusable, easy for Copilot to discover, and less likely to misfire or produce inconsistent behavior.

Use local prompt-library conventions from [README.md](./README.md) and neighboring `*.prompt.md` files as the structure template for this prompt. Use current official VS Code and GitHub Copilot documentation as the best-practice baseline when creating or refining skills.

Remember: this prompt file lives in the prompts folder, but the resulting skill must live in a skill directory, not beside this prompt.

## Inputs

- The user's natural-language request
- An optional existing skill folder, `SKILL.md` path, or skill name
- An optional target scope: workspace/project or user/personal
- Existing skill directories and supporting files such as scripts, templates, examples, and referenced resources
- Current official documentation for Copilot customizations, custom agents, prompt files, and Agent Skills

Treat the user's plain-language request as sufficient input. Do not require `file=`, `scope=`, or other parameter syntax when the intent is already clear.

## Skill fit check

Before editing anything, confirm that a skill is the right primitive:

- Use a **skill** for a reusable, multi-step capability that may include scripts, templates, examples, or other bundled resources.
- Use a **prompt file** for a lightweight single-task slash command with mostly prompt text.
- Use a **custom agent** for a persistent persona, tool restrictions, handoffs, or model preferences.
- Use **instructions** for always-on rules or path-based coding standards.

If another primitive is clearly a better fit, say so briefly. If the user explicitly asked for a skill, continue unless doing so would be misleading or impossible.

## Scenario selection

Choose exactly one primary scenario.

### Scenario 1 — Create one or more new skills

Use this scenario when the user describes a new capability or requests a new reusable workflow.

- If the request contains multiple independent capabilities, create one skill per capability instead of combining unrelated workflows into one oversized skill.
- Infer the skill name, location, and supporting files from the user's request.
- If the skill directory does not exist yet, create the directory and `SKILL.md`.
- Only add supporting scripts, templates, or examples when they materially improve the skill.

### Scenario 2 — Improve existing skill files

Use this scenario when:

- the user names an existing `SKILL.md` file,
- the user names a skill directory or skill name,
- the active file is a `SKILL.md`, or
- the user simply asks to improve skills without naming a specific file.

Rules for this scenario:

- If one skill is named, improve only that skill unless the user asks for broader changes.
- If no specific skill is named, read all `SKILL.md` files in the chosen scope and improve them systematically.
- If no skill files exist in the chosen scope, say so and switch to **Scenario 1** when the request implies creating a new skill.

## Required workflow

1. **Search online first for current Copilot customization best practices.**
   - Start with official VS Code and GitHub Copilot docs.
   - Specifically verify current guidance for prompt files, custom agents, and Agent Skills so the skill is not used for the wrong job.
   - Prefer official sources for file locations, frontmatter fields, discovery behavior, and supported features.
   - Use non-official sources only when they add something materially useful.

2. **Inspect existing skill templates before writing.**
   - Read the target skill if it exists.
   - Review at least 2 relevant skill examples in the selected scope.
   - If the workspace has no relevant skills, use official examples or trusted local reference skills as templates.
   - Read any supporting files in the target skill directory that the skill body references.

3. **Determine the target, scope, and scenario.**
   - Identify the exact skill directory or directories to create or improve.
   - Confirm whether the skill is workspace-scoped or user-scoped.
   - If no skill is named, explain what scope was chosen and why.
   - Keep the default scope narrow unless the user explicitly asks for a broader pass.

4. **Author or improve the skill for discoverability and reliability.**
   - Ensure the skill lives in a directory named after the skill.
   - Ensure `SKILL.md` exists at the skill directory root.
   - Use valid YAML frontmatter.
   - Ensure the `name` is lowercase, hyphenated, matches the parent directory name, and stays concise.
   - Write a `description` that clearly states both **what the skill does** and **when to use it**. Include concrete trigger phrases and keywords so Copilot can discover it reliably.
   - Add `argument-hint` when manual slash-command use would benefit from clearer inputs.
   - Use `user-invocable` and `disable-model-invocation` deliberately:
     - omit both for general-purpose skills,
     - use `user-invocable: false` for auto-loaded background skills,
     - use `disable-model-invocation: true` for manual-only skills.
   - Write a clear body that covers:
     - what the skill helps accomplish,
     - when to use it,
     - step-by-step workflow,
     - expected inputs and outputs,
     - references to bundled scripts, examples, or resources when present,
     - guardrails and failure handling.

5. **Reduce ambiguity and failure modes.**
   - Remove vague or overlapping instructions.
   - Prefer explicit decision rules over soft language.
   - Tell the AI what to do when inputs are missing, incomplete, or ambiguous.
   - Keep unrelated concerns out of the skill.
   - If the skill references commands or resources, ensure the references are real and the links are correct.
   - Do not create empty example folders or placeholder scripts with no operational value.

6. **Keep changes tightly scoped.**
   - Create or edit only the relevant skill directories unless the user explicitly asked to improve all skills.
   - Do not rewrite unrelated customizations for style consistency alone.
   - Preserve the existing skill purpose unless the user explicitly asks to change it.

7. **Validate the result.**
   - Ensure the parent directory name matches the `name` field exactly.
   - Ensure the file name is `SKILL.md`.
   - Ensure frontmatter is valid YAML and uses only supported fields.
   - Ensure the body is internally consistent and easy to follow.
   - Ensure relative links and referenced files exist.
   - Ensure the skill is clearer, more discoverable, and less error-prone than before.

## Skill-writing standards

Apply these standards when authoring or improving skills:

- Prefer concise, imperative instructions.
- Keep responsibilities narrow; split unrelated workflows into separate skills.
- Use numbered procedures when order matters.
- Include examples only when they materially reduce ambiguity.
- Make safety and scope boundaries explicit.
- Prefer real trigger words in the description over clever phrasing.
- Because skills load progressively, make the `name` and especially the `description` strong enough to act as the discovery surface.
- Bundle scripts or templates only when they are part of the workflow, not as decoration.
- Reuse existing local patterns before inventing a new style.

## Output

Report the result in this order:

- **Scenario chosen**
- **Skill fit and scope**
- **Best-practice references used**
- **Target skill file(s)**
- **What changed or was created**
- **Why the skill is more reliable**
- **Assumptions or follow-ups**

## Success criteria

- The selected scenario matches the user's intent.
- Each skill uses the correct directory structure and `SKILL.md` format.
- The frontmatter is valid and the `description` is specific enough for reliable discovery.
- The instructions are clear, repeatable, and resistant to ambiguous execution.
- The changes stay within the requested scope.
- The resulting skill follows current Copilot best practices without unnecessary complexity.
