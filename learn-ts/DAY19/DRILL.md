# DAY19: Drill — "Declaration Files"

Write TypeScript files in `DAY19/`.

## File structure

```
DAY19/
  index.ts          — main entry, uses the ambient types
  index.html     — HTML that loads a JS global
  types/          — directory for declaration files
    globals.d.ts    — ambient declarations
```

## Requirements

### types/globals.d.ts
```typescript
// 1. Declare a global constant APP_NAME: string
// 2. Declare a global function formatDate(date: Date): string
// 3. Declare a global class Counter with:
//    - constructor(startFrom?: number)
//    - increment(): number
//    - reset(): void
// 4. Declare a module "./math-utils" with:
//    - export function square(n: number): number
//    - export const PI: number
```

### index.ts
```typescript
// 1. Use APP_NAME and formatDate — just log them
// 2. Create a Counter instance, increment it a few times, log the value
// 3. Use an import type from "./math-utils" (we won't have the actual JS,
//    so use `import type` or just declare the types inline)
// 4. Demonstrate that TypeScript knows the types
```

### HTML note
Since globals come from script tags, create a note in comments explaining that in a real project, the JS file would be loaded via `<script>` tag before the TypeScript.

## Example output

```
APP_NAME: LearnTS
Formatted date: 2024-01-15
Counter: 1
Counter: 2
Counter: 3
After reset: 0
Square of 5: 25
```

## Hints

<details>
<summary>Click for hints</summary>

- The `declare` keyword tells TypeScript "this exists, trust me"
- Global declarations go outside any `module` or `namespace` block
- To test without actual JS, you can implement simple versions inline using a type assertion
- For the module declaration, you'll need to create a `math-utils.d.ts` or include it in the globals file
- Use `/// <reference path="./types/globals.d.ts" />` or configure `tsconfig.json` to include the types directory
</details>

## Bonus

Write a module augmentation for the built-in `Array` type that adds a `.last()` method returning `T | undefined`.

## How to run

```bash
cd ~/Dev/learn-ts/DAY19
npx tsx index.ts
```

## When you're done

Show me your files.
