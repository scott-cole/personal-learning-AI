# DAY16: Drill — "Type Transformations"

Write a TypeScript program in `DAY16/index.ts`.

## Requirements

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
  createdAt: Date;
}

// 1. Create a mapped type `Serialized<T>` that converts all values to string
//    Optional: use template literal type: `${T[K]}` — but for simplicity, just string

// 2. Create a mapped type `Nullable<T>` that adds null to each value

// 3. Create a mapped type `Jsonify<T>` that converts Date to string, everything else stays

// 4. Create a mapped type `Getters<T>` that creates { getFoo(): T[Foo] } for each property
//    e.g., Getters<User> → { getId(): number; getName(): string; ... }

// 5. Demonstrate each mapped type with console.log

// 6. Write a utility function `serialize<T>(obj: T): Serialized<T>` that converts an object
```

## Example output

```
User: { id: 1, name: "Alice", email: "alice@example.com", isActive: true, createdAt: Date }

Serialized: { id: "1", name: "Alice", email: "alice@example.com", isActive: "true", createdAt: "2024-..." }

Nullable: { id: number | null, name: string | null, ... }

Getters — access:
  getName(): Alice
  getEmail(): alice@example.com
```

## Hints

<details>
<summary>Click for hints</summary>

- For `Serialized<T>`, map each property to `string`
- For `Nullable<T>`, map each property to `T[K] | null`
- For `Getters<T>`, use `get${Capitalize<K & string>}` with template literal types and `Capitalize`
- `Capitalize` is a built-in utility type that capitalizes the first letter
- For `Jsonify<T>`, use a conditional: `T[K] extends Date ? string : T[K]`
- The `serialize` function implementation needs to actually convert values
</details>

## Bonus

Create a `DeepReadonly<T>` mapped type that recursively applies `readonly` to nested objects.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY16
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
