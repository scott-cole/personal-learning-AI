# DAY22: Node.js + TypeScript Setup (ts-node, tsx, esbuild)

---

## "Node + TS is a hybrid car"

A hybrid car has both a gasoline engine and an electric motor. The electric motor handles stop-and-go traffic (development — quick feedback). The gas engine handles the highway (production — optimized output).

TypeScript on Node.js is the same hybrid. During development, you run TypeScript directly with `tsx` — no compile step, instant feedback. For production, you compile to JavaScript with `tsc` or bundle with `esbuild` for optimal performance.

---

## Option 1: tsx (recommended for development)

`tsx` is a modern replacement for `ts-node`. It uses `esbuild` under the hood for lightning-fast execution:

```bash
npm install -D typescript @types/node
npm install -D tsx
```

Run any `.ts` file directly:

```bash
npx tsx src/index.ts
```

No config needed — it picks up your `tsconfig.json` automatically.

---

## Option 2: ts-node (the older standard)

```bash
npm install -D ts-node
npx ts-node src/index.ts
```

`ts-node` is slower than `tsx` because it uses TypeScript's compiler API instead of esbuild. Stick with `tsx` for new projects.

---

## Option 3: compile + run (production)

```bash
npx tsc   # compiles all .ts to .js
node dist/src/index.js  # runs the compiled output
```

Add these scripts to `package.json`:

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/src/index.js"
  }
}
```

`tsx watch` restarts the process when files change — like nodemon for TypeScript.

---

## Option 4: esbuild for speed

esbuild compiles TypeScript to JavaScript in milliseconds (vs seconds for tsc):

```bash
npm install -D esbuild
npx esbuild src/index.ts --bundle --outfile=dist/index.js --platform=node
```

esbuild does **no type checking** — it just strips types. Use `tsc --noEmit` for type checking, then esbuild for the actual compilation.

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "build": "tsc --noEmit && esbuild src/index.ts --bundle --outfile=dist/index.js --platform=node",
    "dev": "tsx watch src/index.ts"
  }
}
```

---

## package.json setup

```json
{
  "name": "my-ts-project",
  "type": "module",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/src/index.js",
    "typecheck": "tsc --noEmit"
  },
  "devDependencies": {
    "typescript": "^5.5.0",
    "tsx": "^4.0.0",
    "@types/node": "^20.0.0"
  }
}
```

---

## tsconfig.json for Node.js

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*.ts"]
}
```

---

## Summary

| Tool | Use | Command |
|------|-----|---------|
| `tsx` | Dev — run directly | `npx tsx src/index.ts` |
| `tsc` | Compile to JS | `npx tsc` |
| `esbuild` | Fast bundle | `npx esbuild src/index.ts --bundle` |
| `tsc --noEmit` | Type-check only | `npx tsc --noEmit` |
| `tsx watch` | Auto-restart on change | `npx tsx watch src/index.ts` |

Your turn. Open DAY22/DRILL.md.
