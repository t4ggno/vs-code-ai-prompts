---
description: Explain complex code in a grounded, easy-to-follow way
agent: agent
argument-hint: selection, symbol, or file path
---

# Explain: Complex Logic

## Goal

Explain a complex block of code in plain language without oversimplifying the parts that matter.

## Inputs

- Current selection, file path, or symbol to explain
- Preferred explanation level if provided (ELI5, junior developer, senior developer, etc.)

If no target is provided and there is no useful selection, ask for the exact code region to explain.

## Required workflow

1. Read the target code and enough surrounding context to understand its purpose.
2. Start with the big picture before diving into details.
3. Explain the flow in the order the code actually executes.
4. Call out important concepts, invariants, edge cases, and data transformations.
5. If helpful, translate the logic into concise pseudocode or a mental model.
6. Base every claim on the actual code; do not guess about missing pieces.

## Constraints

- Do not paraphrase every line if a higher-level explanation is clearer.
- Do not invent business context that is not present in the code.
- Keep the explanation grounded, structured, and beginner-friendly by default.

## Output

Use this structure:

- **One-sentence purpose**
- **Inputs, outputs, and side effects**
- **Step-by-step flow**
- **Key concepts / tricky parts**
- **Edge cases / gotchas**
- **Short mental model or pseudocode** (only if helpful)

## Success criteria

- A developer unfamiliar with the code can follow the explanation.
- The explanation is faithful to the actual implementation.
- The tricky parts are easier to reason about after reading it.
