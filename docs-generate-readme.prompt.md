---
description: Generate or update README content grounded in the actual code
agent: agent
argument-hint: module path, component path, or current selection
---

# Docs: Generate README Content

## Goal

Create a clear README section or README update that accurately explains what the selected module, component, or project does and how to use it.

## Inputs

- Target module, folder, component, or project
- Existing README or template to extend, if present
- Any audience or context constraints (internal team, library users, API consumers, etc.)

If the requested scope is unclear, ask for the exact target rather than inventing documentation scope.

## Required workflow

1. Inspect the target code, exports, configuration, and neighboring docs before writing.
2. If an existing README or template exists, match its tone, heading depth, and structure.
3. Document only what is evidenced in code or explicitly provided by the user.
4. Prioritize the information developers actually need:
   - purpose
   - when to use it
   - key inputs, props, or configuration
   - minimal usage example
   - caveats or constraints
5. If the user asked to update files, edit the README directly. If the user asked only for draft content, provide the markdown draft.

## Constraints

- Do not invent installation steps, scripts, props, environment variables, or behavior.
- Keep the documentation concise and skimmable.
- Use code blocks only for real usage examples.
- Do not overwrite unrelated README sections.

## Output

Use this structure:

- **Documentation target**
- **Section outline**
- **Content created or updated**
- **Files changed**
- **Open questions / missing facts**

## Success criteria

- The README content is accurate, concise, and directly usable.
- A developer can understand what the target is, why it exists, and how to use it.
- The content matches the codebase’s documentation style.
