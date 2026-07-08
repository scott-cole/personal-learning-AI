# DAY20: tsconfig.json Configuration

---

## "tsconfig is a control panel"

Your kitchen has a control panel — a thermostat for the oven, a dimmer for the lights, a switch for the garbage disposal, a timer for the coffee maker. Each knob and switch controls how a different appliance behaves.

`tsconfig.json` is the control panel for the TypeScript compiler. Every toggle changes how strict, fast, or compatible the compiler is.

---

## Creating tsconfig.json

```bash
npx tsc --init
```

This generates a `tsconfig.json` with all options commented out. Let's look at the essential ones.

---

## The essentials

```json
{
  "compilerOptions": {
    "target": "ES2022",           // Which JS version to emit
    "module": "ESNext",           // Module system for output
    "moduleResolution": "bundler",// How to resolve imports
    "outDir": "./dist",           // Where to put compiled JS
    "rootDir": "./src",           // Where source files live
    "strict": true                // Enable all strict checks
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules", "dist"]
}
```

---

## target — which JavaScript version?

| Target | Features | Browser support |
|--------|----------|----------------|
| `ES5` | IE-compatible, no arrow functions | Very old |
| `ES2020` | Optional chaining, nullish coalescing | Modern |
| `ES2022` | Class fields, top-level await | Very modern |
| `ESNext` | Latest proposals | Bleeding edge |

For Node.js 18+, use `"target": "ES2022"`. For modern browsers, `"ES2020"`.

---

## module — how imports work

| Module | Import/export style | Use case |
|--------|--------------------|----------|
| `ESNext` | `import`/`export` | Bundlers (Vite, webpack) |
| `CommonJS` | `require()`/`module.exports` | Node.js without ESM |
| `NodeNext` | ESM with Node resolution | Node.js ESM projects |

---

## strict mode — the seatbelt

```json
{
  "strict": true
}
```

This enables **all** of these at once:

| Option | What it does |
|--------|-------------|
| `strictNullChecks` | `null`/`undefined` aren't assignable to everything |
| `noImplicitAny` | Error if type can't be inferred |
| `strictFunctionTypes` | Stronger function type checking |
| `strictPropertyInitialization` | Class properties must be initialized |
| `noUncheckedIndexedAccess` | Accessing `Record` values might be undefined |

**Always use `strict: true`.** Without it, TypeScript is barely stricter than JavaScript.

---

## Path aliases

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

Then in code:

```typescript
import { greet } from "@/utils/greet";
// instead of: import { greet } from "../../../utils/greet";
```

---

## Source maps

```json
{
  "compilerOptions": {
    "sourceMap": true
  }
}
```

Generates `.js.map` files so your debugger shows TypeScript source, not compiled JavaScript.

---

## Common project setups

**Library:**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "declaration": true,    // generate .d.ts files
    "outDir": "./dist",
    "strict": true,
    "sourceMap": true
  }
}
```

**Node.js app:**
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

## Summary

| Option | Purpose |
|--------|---------|
| `target` | Output JS version |
| `module` | Import/export system |
| `outDir` | Output directory |
| `rootDir` | Source directory |
| `strict` | Enable all type checks |
| `declaration` | Generate `.d.ts` |
| `sourceMap` | Debug in TypeScript |
| `paths` | Import aliases |
| `include`/`exclude` | File selection |

Your turn. Open DAY20/DRILL.md.
