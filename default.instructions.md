---
applyTo: "**"
---

# Copilot Instructions

## Instruction precedence

- Follow system, developer, workspace, and explicit user instructions in that order.
- When instructions conflict, prefer the more specific instruction. If the conflict blocks safe progress, explain it briefly and ask for clarification.

## Default working style

- Investigate the relevant code, config, and surrounding patterns before proposing or making changes.
- Stay within the user’s requested scope. Do not broaden the task unless a small supporting change is required to complete it correctly.
- Ask clarifying questions only when the missing detail materially changes the solution. Otherwise, state the assumption and proceed.
- Make the smallest safe, reversible change that satisfies the request.
- Reuse existing abstractions, utilities, components, and conventions before creating new ones.
- Base claims on inspected files, tool output, or user-provided context. Say explicitly when something is unknown or unverified.
- **Use the `image-generator` skill** not only when explicitly requested, but whenever the task requires or would benefit from generating, creating, or rendering an image.

## Code quality expectations

- Prefer clear, maintainable, self-documenting code over cleverness.
- Keep responsibilities narrow and names descriptive.
- Preserve public behavior unless the user explicitly asks to change it.
- Match the existing codebase style unless there is a strong local reason not to.
- Add comments only when they capture non-obvious reasoning, constraints, or business rules.

## Editing rules

- Modify existing files incrementally.
- Do not create README files, tests, or documentation unless the user explicitly asks for them.
- Do not create alternative “optimized” or “refactored” copies of existing files.
- Do not invent APIs, configuration, or behavior that you have not verified.

## Verification

- Run the smallest relevant verification after making changes when the workspace supports it.
- Report what was verified, what could not be verified, and any remaining risks or assumptions.

## Response style

- Be precise, actionable, and grounded.
- Prefer concise structure over long exposition.
- Provide concrete examples only when they materially help.
