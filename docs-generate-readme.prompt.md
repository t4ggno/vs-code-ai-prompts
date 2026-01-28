# Docs: Generate README Section

**Goal**: Create a high-quality, informative README section for a module, component, or project.

## Context
A good README serves as the entry point for other developers. It should answer "What is this?", "Why should I use it?", and "How do I use it?".

## Instructions

1.  **Analyze Context**: Read the provided file(s) or folder structure to understand the module's purpose.
2.  **Structure the Content**:
    *   **Title**: Clear and descriptive.
    *   **Description**: One-paragraph summary of functionality.
    *   **Features**: Bulleted list of key capabilities.
    *   **Installation/Usage**: Code blocks showing how to import and use it.
    *   **API / Props**: detailed table or list of arguments/props.
    *   **Notes**: Any caveats, constraints, or dependencies.
3.  **Use Markdown Best Practices**:
    *   Use correct header levels (`##` for sections).
    *   Use syntax highlighting for code blocks (e.g., ````typescript`).
    *   Keep it concise but complete.

## Output Format

```markdown
## [Module Name]

[Concise description of the module]

### Features
- [Feature 1]
- [Feature 2]

### Usage

```typescript
import { Module } from './path';

const instance = new Module();
```

### Props / Configuration

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| name | string | - | The name of the item |

```

## Example Usage

**Request:** "Generate a README docs for `src/components/Button.tsx`"

**Output:**
```markdown
## Button Component

A strict, accessible button component that adheres to the design system. Supports various variants (primary, secondary) and sizes.

### Usage

```tsx
import { Button } from '@/components/Button';

<Button variant="primary" onClick={handleClick}>
  Submit
</Button>
```

### Props

| Name | Type | Default | Description |
|------|------|---------|-------------|
| variant | `'primary' | 'secondary'` | `'primary'` | Visual style of the button |
| onClick | `() => void` | - | Click handler |
```
