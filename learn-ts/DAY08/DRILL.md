# DAY08: Drill — "Generic Collections"

Write a TypeScript program in `DAY08/index.ts`.

## Requirements

```typescript
// 1. Write a generic function `reverse<T>(items: T[]): T[]`
//    that returns a new array with elements in reverse order

// 2. Write a generic function `first<T>(items: T[], defaultVal: T): T`
//    that returns the first element or the default value if empty

// 3. Write a generic interface `KeyValuePair<K, V>` with:
//    - key: K
//    - value: V

// 4. Write a generic class `Repository<T>` with:
//    - private items: Map<string, T>
//    - add(id: string, item: T): void
//    - get(id: string): T | undefined
//    - getAll(): T[]
//    - remove(id: string): boolean

// 5. Create a Repository<User> where User has id, name, email
//    Add 3 users, get one by id, list all, remove one
```

## Example output

```
reverse([1, 2, 3]) → [3, 2, 1]
reverse(["a", "b"]) → ["b", "a"]

first([], 0) → 0
first([10, 20], 0) → 10

Users:
  user_1: Alice (alice@example.com)
  user_2: Bob (bob@example.com)
  user_3: Charlie (charlie@example.com)

Get user_2: Bob (bob@example.com)
Remove user_1: true
Remaining: 2 users
```

## Hints

<details>
<summary>Click for hints</summary>

- For `reverse`, return `[...items].reverse()` (copy first to avoid mutation)
- `Map` has `.set(key, val)`, `.get(key)`, `.has(key)`, `.delete(key)`
- Use `Array.from(this.items.values())` or spread `[...this.items.values()]` for `getAll()`
- Define the `User` interface at the top of the file
</details>

## Bonus

Add a generic constraint `T extends { id: string }` to `Repository` and implement `findById` that uses the `id` property instead of a separate key param.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY08
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
