# Design: Scaffold Feature

**Goal**: Plan and generate the file structure and interfaces for a new feature before implementing detailed logic.

## Context
"Measure twice, cut once." Scaffolding ensures that the architecture is sound and consistent with the project structure before writing code.

## Instructions

1.  **Analyze Requirements**: Break down the feature request into technical components (UI, Logic, Data, API).
2.  **Define Architecture**:
    *   List necessary files (Components, Hooks, Services, Types).
    *   Define directory structure.
3.  **Draft Interfaces (API Contract)**:
    *   Define the `interface` or `type` for data models.
    *   Define function signatures for services/utils.
4.  **Generate Scaffold Code**:
    *   Create "stub" files with imports, exports, and types, but empty bodies (or `TODO` comments).

## Output Format

### 📂 Directory Structure
```
src/features/user-profile/
├── components/
│   └── ProfileCard.tsx
├── hooks/
│   └── useProfile.ts
├── types/
│   └── index.ts
└── api/
    └── userApi.ts
```

### 📝 Key Interfaces (src/features/user-profile/types/index.ts)
```typescript
export interface UserProfile {
  id: string;
  avatarUrl?: string;
  // ...
}
```
