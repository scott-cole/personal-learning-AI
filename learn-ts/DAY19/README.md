# DAY19: Declaration Files and Ambient Types

---

## "Declaration files are a legend on a map"

A hiking map has a legend: solid line for trails, dashed for dirt roads, triangle for campground, star for ranger station. The legend tells you what each symbol *means* without being the actual trail.

Declaration files (`.d.ts`) are a legend for JavaScript code. They describe the *types* of existing JavaScript code without reimplementing it. The actual implementation is the trail; the declaration file is the map.

---

## What is a .d.ts file?

When you write TypeScript, the compiler generates type information. But when you use a plain JavaScript library, TypeScript needs a description of its types. That's what `.d.ts` files provide.

```typescript
// greeter.d.ts
export function greet(name: string): string;
export const VERSION: string;
```

This file contains only type information — no implementation. It tells TypeScript "trust me, there's a `greet` function that takes a string and returns a string."

---

## Ambient declarations with `declare`

```typescript
// globals.d.ts
declare const API_KEY: string;
declare function logToServer(message: string): void;
declare class ApiClient {
  constructor(baseUrl: string);
  get<T>(path: string): Promise<T>;
}
```

These tell TypeScript that `API_KEY`, `logToServer`, and `ApiClient` exist in the global scope — typically added by a script tag in the browser or a Node.js global.

---

## Ambient modules

```typescript
// my-library.d.ts
declare module "my-library" {
  export function doSomething(input: string): number;
  export const defaultConfig: Record<string, string>;
}
```

This lets you `import { doSomething } from "my-library"` even though the library is JavaScript. TypeScript knows the types from the declaration.

---

## Augmenting existing modules

```typescript
// augmentations.d.ts
import "express";

declare module "express" {
  interface Request {
    user?: {
      id: number;
      name: string;
    };
  }
}
```

This adds a `user` property to Express's `Request` type. TypeScript merges this declaration with the original.

---

## DefinitelyTyped (@types packages)

Most popular libraries have types on npm under `@types/`:

```bash
npm install --save-dev @types/node
npm install --save-dev @types/express
npm install --save-dev @types/lodash
```

TypeScript automatically finds these. When you `import` a module, it looks for:
1. The module's own bundled types (`.d.ts` in the package)
2. A matching `@types/` package
3. Your local declarations

---

## Writing your own declaration file

```typescript
// utils.d.ts
export function formatCurrency(amount: number, currency?: string): string;

export interface Config {
  theme: "light" | "dark";
  language: string;
}
```

If you're writing TypeScript, you don't need `.d.ts` files — the `.ts` file *is* the declaration. `.d.ts` is only for describing JavaScript code.

---

## Triple-slash directives

Legacy way to reference other type files:

```typescript
/// <reference path="./legacy-types.d.ts" />
/// <reference types="node" />
```

Modern TypeScript uses module imports instead. Triple-slash is only needed for global declarations or legacy projects.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Type-only declaration | `export function foo(x: string): void;` in `.d.ts` |
| Ambient declaration | `declare const x: number;` |
| Module declaration | `declare module "lib" { ... }` |
| Module augmentation | `declare module "lib" { interface X { newProp: T } }` |
| Type package | `npm install @types/library` |

Your turn. Open DAY19/DRILL.md.
