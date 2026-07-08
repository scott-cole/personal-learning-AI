# DAY15: Drill — "Type-Safe Object Utilities"

Write a TypeScript program in `DAY15/index.ts`.

## Requirements

```typescript
// 1. Write a function `pluck<T, K extends keyof T>(objs: T[], key: K): T[K][]`
//    - Extracts a single property from each object in an array
//    - e.g., pluck(users, "name") → string[]

// 2. Write a function `pick<T, K extends keyof T>(obj: T, keys: K[]): Pick<T, K>`
//    - Returns a new object with only the specified keys
//    - Hint: use reduce

// 3. Write a function `hasKey<T>(obj: T, key: PropertyKey): key is keyof T`
//    - Returns true if the key exists in the object (use `in` operator)

// 4. Test with an interface Product with:
//    id: number, name: string, price: number, category: string
```

## Example output

```
Products: [
  { id: 1, name: "Laptop", price: 999, category: "Electronics" },
  { id: 2, name: "Shirt", price: 29, category: "Clothing" },
]

pluck(products, "name") → ["Laptop", "Shirt"]
pluck(products, "price") → [999, 29]

pick(products[0], ["name", "price"]) → { name: "Laptop", price: 999 }

hasKey(products[0], "category") → true
hasKey(products[0], "color") → false
```

## Hints

<details>
<summary>Click for hints</summary>

- `Array<Pick<T, K>>` is an alternative to `T[K][]`
- For `pick`, use `Object.entries` or manual iteration
- For `hasKey`, the return type `key is keyof T` is a **type predicate** — it narrows the type for the caller
- `.reduce((acc, key) => ({ ...acc, [key]: obj[key] }), {} as Pick<T, K>)`
</details>

## Bonus

Write `pick` as a generic that also accepts `(keyof T)[]` and returns the narrowed type correctly. Write `omit<T, K extends keyof T>(obj: T, keys: K[]): Omit<T, K>`.

## How to run

```bash
cd ~/Dev/learn-ts/DAY15
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
