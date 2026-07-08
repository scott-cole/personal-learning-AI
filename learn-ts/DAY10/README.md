# DAY10: Modules, Imports, and Exports

---

## "Modules are a filing cabinet"

Your desk is cluttered with papers — receipts, contracts, notes, photos. You can't find anything. Then you buy a filing cabinet. Drawers for each category. Folders inside each drawer. Labels on every tab.

Modules are a filing cabinet for your code. Each file is a drawer. `export` decides what goes in labelled folders. `import` pulls a folder out when you need it.

---

## Exporting — labelling the folders

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

export const PI = 3.14159;

export interface Point {
  x: number;
  y: number;
}
```

Anything with `export` is visible to other files. Everything else is **private** to that file.

---

## Importing — pulling folders out

```typescript
// main.ts
import { add, PI, Point } from "./math";

console.log(add(2, 3)); // 5
console.log(PI);        // 3.14159
```

The `{ }` is **named import**. You pick exactly what you need.

---

## Default exports — the main drawer

Each module can have one `export default`. This is often used for the main class or function in a file:

```typescript
// logger.ts
export default function log(message: string): void {
  console.log(`[LOG]: ${message}`);
}

export function warn(message: string): void {
  console.warn(`[WARN]: ${message}`);
}
```

```typescript
// main.ts
import log, { warn } from "./logger";

log("App started");    // default import — no braces
warn("Low memory");   // named import — braces
```

---

## Renaming imports

Avoid naming collisions or shorten names:

```typescript
import { add as sum } from "./math";
import * as MathUtils from "./math";

console.log(sum(1, 2));          // 3
console.log(MathUtils.PI);       // 3.14159
console.log(MathUtils.add(1, 2)); // 3
```

`import * as` gives you a namespace object with all exports.

---

## Re-exporting

A file can gather and re-export from multiple files — like a summary drawer:

```typescript
// index.ts
export { add, subtract } from "./math";
export { User, type UserRole } from "./user";
export { log } from "./logger";
```

This is the **barrel pattern** — a single entry point for a folder of modules.

---

## Type-only imports/exports

When you only need the type, use `import type` or `export type`:

```typescript
import type { User } from "./user";
import { formatUser, type User } from "./user"; // mixed
```

This tells the bundler/compiler it can safely remove the import if only used as a type — it's purely for type-checking.

---

## Module resolution

TypeScript resolves `"./math"` by looking for:
1. `./math.ts`
2. `./math/index.ts`
3. `./math.d.ts`

Always use relative paths (starting with `./` or `../`) for your own files. Use bare specifiers for npm packages (`"lodash"`, `"express"`).

---

## Summary

| Concept | Syntax |
|---------|--------|
| Named export | `export function foo()` |
| Named import | `import { foo } from "./bar"` |
| Default export | `export default class Foo {}` |
| Default import | `import Foo from "./bar"` |
| Rename import | `import { foo as bar }` |
| Namespace import | `import * as Utils from "./bar"` |
| Type import | `import type { Foo } from "./bar"` |

Your turn. Open DAY10/DRILL.md.
