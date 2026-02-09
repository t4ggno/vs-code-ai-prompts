---
applyTo: "**"
---

# Modern TypeScript Cheatsheet (2026)

## Type System Essentials

### Modern Type Definitions

```typescript
// Utility types - modern approach
type User = {
  readonly id: string;
  name: string;
  email?: string;
} & Record<string, unknown>;

// Template literal types
type EventName = `on${Capitalize<string>}`;
type Status = "loading" | "success" | "error";

// Conditional types with constraints
type NonNullable<T> = T extends null | undefined ? never : T;
```

### Advanced Generics

```typescript
// Generic constraints with keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Mapped types with modifiers
type Optional<T> = { [K in keyof T]?: T[K] };
type DeepReadonly<T> = { readonly [K in keyof T]: DeepReadonly<T[K]> };
```

## Narrowing & Type Guards

```typescript
// typeof guards
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}

// in-operator guards
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    animal.swim();
    return;
  }
  animal.fly();
}

// user-defined type predicates
const isFish = (pet: Fish | Bird): pet is Fish => "swim" in pet;
```

## Discriminated Unions & Exhaustiveness

```typescript
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; sideLength: number };
type Shape = Circle | Square;

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default: {
      const _exhaustive: never = shape;
      return _exhaustive;
    }
  }
}
```

## Assertion Functions

```typescript
function assertIsString(value: unknown): asserts value is string {
  if (typeof value !== "string") {
    throw new TypeError("Expected string");
  }
}

function shout(value: unknown) {
  assertIsString(value);
  return value.toUpperCase();
}
```

## this-based Type Guards (Classes)

```typescript
class Box<T> {
  value?: T;

  hasValue(): this is { value: T } {
    return this.value !== undefined;
  }
}

const box = new Box<string>();
if (box.hasValue()) {
  box.value.toUpperCase();
}
```

## Performance Best Practices

### Type Assertions (Avoid Runtime Cost)

```typescript
// ✅ Good - Type assertion
const element = document.getElementById("app") as HTMLElement;

// ✅ Better - Type guards
const isString = (value: unknown): value is string => typeof value === "string";

// ❌ Avoid - Runtime type checking when not needed
```

### Efficient Array/Object Operations

```typescript
// ✅ Destructuring with known structure
const { id, name, ...rest } = user;

// ✅ Object.entries with proper typing
Object.entries(obj as Record<string, unknown>).forEach(([key, value]) => {});

// ✅ Array methods with type inference
const ids = users.map(({ id }) => id);
const active = users.filter(
  (user): user is ActiveUser => user.status === "active",
);
```

## Modern Features (TypeScript 5.0+)

### const assertions & satisfies

```typescript
// ✅ const assertions for immutable data
const config = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
} as const;

// ✅ satisfies operator for type checking
const theme = {
  colors: { primary: "#007acc" },
  spacing: { small: 4 },
} satisfies Theme;
```

### Decorators & Metadata

```typescript
// ✅ Modern decorator syntax (Stage 3)
class UserService {
  @Cache({ ttl: 60000 })
  async getUser(id: string): Promise<User> {}
}
```

### Import/Export Patterns

```typescript
// ✅ Type-only imports (tree-shaking friendly)
import type { User } from "./types";
import { validateUser } from "./utils";

// ✅ Named exports (better for tree-shaking)
export const userService = new UserService();
export type { User, CreateUserDto };
```

## Async/Performance Patterns

### Promise & Async Best Practices

```typescript
// ✅ Promise.all for parallel execution
const [users, posts] = await Promise.all([fetchUsers(), fetchPosts()]);

// ✅ Promise.allSettled for error handling
const results = await Promise.allSettled([...promises]);

// ✅ Typed async generators
async function* processData(): AsyncGenerator<ProcessedItem> {
  for await (const item of dataStream) {
    yield await processItem(item);
  }
}
```

### Error Handling

```typescript
// ✅ Result pattern for explicit error handling
type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };

// ✅ Async error boundaries
const safeAsync = async <T>(fn: () => Promise<T>): Promise<Result<T>> => {
  try {
    const data = await fn();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error as Error };
  }
};
```

## Common Anti-Patterns to Avoid

```typescript
// ❌ Avoid
any, Function, Object, {}, unknown without narrowing

// ⚠️ Prefer unions for simple value sets
// Enums can be useful for bitflags or interop with external APIs
enum Status { Ready, Busy }

// ❌ Avoid runtime type checking for known types
if (typeof data === 'object') // when type is already known

// ❌ Avoid nested conditional types when simpler alternatives exist
type Complex<T> = T extends A ? (T extends B ? C : D) : E;

// ✅ Use union types or mapped types instead
type Simple<T> = T extends A & B ? C : T extends A ? D : E;
```

## Quick Reference

### Utility Types Priority List

1. `Partial<T>`, `Required<T>`, `Pick<T, K>`, `Omit<T, K>`
2. `NonNullable<T>`, `Parameters<T>`, `ReturnType<T>`
3. `Awaited<T>` for async return types
4. `Record<K, V>` for key-value structures

### Performance Tips

- Use `const assertions` for immutable data
- Prefer `interface` over intersections for composed object shapes
- Add explicit return types on public APIs to reduce inference cost
- Name complex conditional/mapped types to improve reuse and readability
- Prefer base types + subtypes over huge unions when modeling large domains
