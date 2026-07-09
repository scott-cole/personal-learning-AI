# DAY20: Drill — "Configure a Project"

Set up a TypeScript project from scratch in `DAY20/`.

## File structure

```
DAY20/
  tsconfig.json     — write it manually (don't use --init)
  src/
    index.ts        — entry point
    utils/
      format.ts     — utility functions
  dist/             — (created by tsc)
```

## Requirements

### tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "sourceMap": true,
    "declaration": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*.ts"]
}
```

### src/utils/format.ts
```typescript
export function capitalize(str: string): string {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

export function truncate(str: string, maxLen: number): string {
  return str.length > maxLen ? str.slice(0, maxLen) + "..." : str;
}
```

### src/index.ts
```typescript
import { capitalize, truncate } from "./utils/format.js";

const name = "hello world";
console.log(capitalize(name));
console.log(truncate("This is a long sentence", 10));
```

## Tasks

```bash
# 1. Create all files manually (no --init)
# 2. Run tsc to compile
# 3. Run the output with node
# 4. Verify .d.ts and .js.map files exist in dist/
# 5. Verify a type error gets caught (add `const x: number = "string"` and recompile)
```

## Example output

```
Hello world
This is a ...
```

## Hints

<details>
<summary>Click for hints</summary>

- Run `npx tsc` in the DAY20 directory — it auto-detects tsconfig.json
- If you get module resolution errors, ensure moduleResolution matches module
- The output files go to `dist/src/utils/format.js` (mirroring the src structure)
- In ESM mode, imports need `.js` extensions even in .ts files (TypeScript convention)
- Run the compiled JS: `node dist/src/index.js`
</details>

## Bonus

Add a `paths` alias in tsconfig.json so you can import `@utils/format` instead of `./utils/format.js`. You'll also need to configure your runtime/bundler to resolve the aliases.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY20
npx tsc
node dist/src/index.js
```

## When you're done

Show me your tsconfig.json and the terminal output.
