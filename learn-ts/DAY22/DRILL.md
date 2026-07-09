# DAY22: Drill — "Project Setup"

Initialize a proper Node.js + TypeScript project in `DAY22/`.

## File structure

```
DAY22/
  package.json
  tsconfig.json
  src/
    index.ts
```

## Requirements

### Setup steps

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY22

# 1. Create package.json manually (or npm init)
# 2. Create tsconfig.json manually
# 3. npm install typescript @types/node tsx --save-dev
# 4. Create src/index.ts
# 5. Add scripts to package.json
```

### package.json
```json
{
  "name": "day22-setup",
  "type": "module",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/src/index.js",
    "typecheck": "tsc --noEmit"
  }
}
```

### src/index.ts
```typescript
import { createServer } from "http";

const hostname = "127.0.0.1";
const port = 3000;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader("Content-Type", "application/json");
  res.end(JSON.stringify({
    message: "Hello from TypeScript + Node!",
    timestamp: new Date().toISOString(),
    url: req.url,
  }));
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### Verification

```bash
# 1. Run with tsx
npx tsx src/index.ts
# Visit http://localhost:3000 in browser

# 2. Type-check
npm run typecheck

# 3. Build
npm run build

# 4. Run compiled output
npm start
```

## Expected output

```
Server running at http://127.0.0.1:3000/

# Browser at http://localhost:3000/ returns:
# { "message": "Hello from TypeScript + Node!", "timestamp": "2024-...", "url": "/" }
```

## Hints

<details>
<summary>Click for hints</summary>

- `"type": "module"` enables ES modules (import/export) in Node.js
- Without `--save-dev`, dependencies go to `dependencies` instead of `devDependencies`
- `tsx watch` auto-restarts on file changes — try editing the message while it's running
- After `npm run build`, check the `dist/src/` directory for compiled JS
</details>

## Bonus

Add a second route: `GET /health` returns `{ status: "ok" }`. Use `req.url` to route.

## When you're done

Show me your package.json, tsconfig.json, and terminal output from all 4 verification steps.
