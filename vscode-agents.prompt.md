---
description: Create a new Copilot custom agent file or improve an existing one using current best practices and local customization-library conventions
agent: agent
argument-hint: describe the agent to create, or name an existing .agent.md file to improve
---

# VS Code: Create or Improve Custom Agents

## Goal

Create a new Copilot custom agent file or improve an existing one so it is clear, consistent, least-privilege, and reliable to run in VS Code.

Use current official documentation as the primary authority, especially:

- [Custom agents in VS Code](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
- [Prompt files in VS Code](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
- [Custom instructions in VS Code](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Use tools with agents](https://code.visualstudio.com/docs/copilot/agents/agent-tools)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/copilot/customization/agent-skills)

Use [README.md](./README.md), [default.instructions.md](./default.instructions.md), and neighboring `vscode-*.prompt.md` files as local style references. If local wording and official custom-agent guidance conflict, prefer the official agent format and then adapt the tone to match the local library.

## Inputs

- The user's natural-language request
- An optional target agent file name or path
- The active file or selection if it is relevant
- Existing custom agent files in the current workspace or active customization scope
- Relevant instruction files such as `AGENTS.md`, `.github/copilot-instructions.md`, or `*.instructions.md` when they help avoid duplication
- Current official guidance about custom agents, prompt files, tools, and instructions

Treat the user's plain-language request as sufficient input. Do not require `file=`, `mode=`, `tools=`, or other parameter syntax when the intent is already clear.

## First decide: is a custom agent the right artifact?

- Use a **custom agent** when the user needs a persistent persona, specific tool restrictions, model preferences, or handoffs between roles.
- Use a **prompt file** for a lightweight slash command or a reusable task that does not need its own persistent persona.
- Use an **agent skill** when the workflow needs scripts, examples, or other reusable resources that should load on demand across environments.
- `AGENTS.md` is an always-on instructions file, not a selectable custom agent. If the user actually wants instructions rather than a custom agent, say so briefly and adjust course only if the intent is clear or the user confirms it.

If a prompt file or skill would be a better fit, say so briefly. If the user still wants an agent, continue and create or improve the agent instead of refusing the task.

## Scenario selection

Choose exactly one primary scenario.

### Scenario 1 — Create a new custom agent

Use this scenario when:

- the user describes a new agent, role, persona, or reusable workflow,
- no existing `.agent.md` file is a good match, or
- no agent files exist and the request is clearly agent-shaped.

### Scenario 2 — Improve existing custom agent file(s)

Use this scenario when:

- the user names an existing `.agent.md` file,
- the active file is an existing agent file,
- the user names a legacy `.chatmode.md` file that should be modernized, or
- the user asks to improve agents without naming a specific file.

If no specific file is named, inspect all relevant agent files in the active scope and improve the ones that materially benefit from current best practices. If no agent files exist, switch to **Scenario 1** instead of forcing an unrelated edit.

## Required workflow

1. **Search online first for current Copilot custom-agent best practices.**
	- Start with the official VS Code documentation linked above.
	- Prefer verified guidance about `.agent.md` files, frontmatter fields, tool restrictions, handoffs, and location rules.
	- Use GitHub documentation only when organization-level or GitHub Copilot-specific behavior is relevant.
	- Use non-official sources only if they add something materially useful that the official docs do not cover.

2. **Inspect existing agents and nearby customizations before writing.**
	- Read the target agent file if one exists.
	- Review at least 2 relevant neighboring agent files as templates.
	- If fewer than 2 agent files exist, review the closest official custom-agent example plus nearby prompt, skill, or instruction files that show the local writing style.
	- Reuse sane naming, structure, and tool patterns instead of inventing a new style.

3. **Determine the target and the scenario.**
	- For **Scenario 1**, infer the agent's purpose, scope, location, and likely file name from the user's request.
	- For **Scenario 2**, identify the agent file or files to improve.
	- If no file is named, explain which existing agent files were selected and why.
	- Ask a focused clarifying question only if the missing detail would materially change the agent's role, file selection, or tool safety.

4. **Draft or improve the agent with current custom-agent rules.**
	- Use valid YAML frontmatter and a `.agent.md` file for VS Code custom agents.
	- Include a concise `description`.
	- Add `argument-hint` when it helps users invoke the agent correctly.
	- Add `tools` only when you can verify the tool names and the agent truly needs them.
	- Use the smallest safe tool list; prefer least privilege over convenience.
	- Use `agents` only when the agent genuinely needs subagents, and include the agent tool if required.
	- Use `handoffs` only when the next step is explicit, valuable, and the target agent exists or is being created in the same task.
	- Use `model` only when a specific model choice materially improves the workflow.
	- Use `user-invocable: false` only for helper or subagent-only agents.
	- Use `disable-model-invocation: true` only when the agent must not be invoked automatically.
	- Never use deprecated `infer`.
	- Add `target` only when the intended runtime actually requires it.

5. **Make the body deterministic and low-ambiguity.**
	- Give the agent one clear role.
	- State when to use it and, when helpful, when not to use it.
	- Define a repeatable workflow with explicit decision rules.
	- Tell the AI what to do when input is missing, ambiguous, or out of scope.
	- Keep the default scope narrow and explicit.
	- Prefer linking existing instruction files over duplicating large rule sets inside the agent.
	- If the agent performs reviews or analysis, require it to separate confirmed findings from assumptions or open questions.

6. **Preserve the right format for the right location.**
	- Prefer workspace agents in `.github/agents/<agent-name>.agent.md`.
	- Preserve the existing location when improving an agent unless the user explicitly asks to move it.
	- If the workspace intentionally uses `.claude/agents`, preserve Claude-format agent files instead of force-migrating them.
	- If you encounter a legacy `.chatmode.md` file, explain that custom chat modes are now custom agents. Rename or migrate only when the user asked for modernization or when the old format would prevent the agent from loading.

7. **Keep the change tightly scoped.**
	- Create or edit only the relevant agent file or files.
	- Do not silently create supporting prompts, instructions, skills, hooks, or MCP configuration unless the user explicitly asks for them.
	- Do not broaden one agent into multiple unrelated personas just because the user mentioned several tasks.
	- Prefer one focused agent per file.

8. **Validate the result.**
	- Ensure the file name is kebab-case when creating a new VS Code agent file.
	- Ensure new VS Code agent files end with `.agent.md`.
	- Ensure frontmatter fields are current, valid, and internally consistent.
	- Ensure linked files and handoff targets actually exist.
	- Ensure tool names are verified or omitted.
	- Ensure the final agent is easier to run consistently and less likely to misinterpret user intent.

## Agent-authoring standards

Apply these standards when authoring or improving an agent:

- Prefer concise, imperative instructions.
- Keep one responsibility per section.
- Keep the agent's role stable across repeated runs.
- Default to least-privilege tools.
- Avoid giant tool lists “just in case.”
- Reuse existing instructions and conventions before inventing new ones.
- Avoid unsupported metadata fields and placeholder text.
- Avoid contradictory rules, overlapping personas, or vague phrases such as “use your judgment” when a clear decision rule is possible.
- Add brief examples only when they materially reduce ambiguity.

## Output

Report the result in this order:

- **Artifact fit**
- **Scenario chosen**
- **Best-practice references used**
- **Target file**
- **What changed**
- **Why this agent is more reliable**
- **Assumptions or follow-ups**

## Success criteria

- The agent matches the user's intended role or workflow.
- The file uses current VS Code custom-agent conventions.
- The instructions are clear, repeatable, and resistant to ambiguous execution.
- The tool configuration follows least-privilege principles.
- The result reuses local conventions where they help without copying irrelevant boilerplate.
- The final file contains no placeholder content, deprecated metadata, or invented capabilities.
