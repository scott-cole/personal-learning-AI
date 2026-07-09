# DAY30: Drill — "Full-Stack Todo Application"

Build the complete full-stack todo app in `DAY30/`.

## Setup

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY30

# Server
mkdir server && cd server
npm init -y
npm install express cors zod
npm install -D typescript @types/node @types/express @types/cors tsx
cd ..

# Client
npm create vite@latest client -- --template react-ts
cd client
npm install
cd ..
```

## File structure

```
DAY30/
  server/
    src/
      index.ts
      routes/
        todos.ts
      types.ts
      schemas.ts
    package.json
    tsconfig.json

  client/
    src/
      App.tsx
      App.css
      types.ts
      hooks/
        useTodos.ts
      components/
        TodoInput.tsx
        TodoList.tsx
        TodoItem.tsx
    package.json
    tsconfig.json
```

## Requirements

### Server

```typescript
// types.ts
export interface Todo {
  id: number;
  title: string;
  completed: boolean;
  createdAt: string;
}

// schemas.ts
import { z } from "zod";
export const CreateTodoSchema = z.object({
  title: z.string().min(1, "Title is required"),
});
export const UpdateTodoSchema = z.object({
  title: z.string().min(1).optional(),
  completed: z.boolean().optional(),
});

// routes/todos.ts
// GET    /api/todos     → Todo[]
// POST   /api/todos     → CreateTodoSchema → Todo
// PUT    /api/todos/:id → UpdateTodoSchema → Todo
// DELETE /api/todos/:id → 204
```

### Client

- `TodoInput`: text input + "Add" button — calls `addTodo`
- `TodoList`: renders list of `TodoItem` components
- `TodoItem`: shows title (strikethrough if completed), checkbox, delete button
- `useTodos` hook: encapsulate all fetch logic
- Loading state: show "Loading..." while fetching
- Empty state: show "No todos yet!" list is empty

## Running both

```bash
# Terminal 1 — Server
cd DAY30/server
npx tsx src/index.ts

# Terminal 2 — Client
cd DAY30/client
npm run dev
```

Open http://localhost:5173 in the browser.

## Hints

<details>
<summary>Click for hints</summary>

- Server runs on port 3001, Vite runs on 5173 — configure Vite proxy or use CORS
- Add `"proxy": "http://localhost:3001"` to client's `vite.config.ts`
- Or add `cors()` middleware on the server
- Use `useEffect` for initial fetch, async functions for mutations
- `crypto.randomUUID()` or `Date.now()` for generating IDs server-side
- After each mutation (add/toggle/delete), update local state optimistically or re-fetch
- Add Zod validation on the server — return 400 with field errors
- `fetch` returns a Response — call `.json()` to get the body
</details>

## Bonus

Add filtering (All / Active / Completed). Add localStorage persistence on the server. Add a "Clear completed" button. Deploy with Docker.

## When you're done

Show me your files, the server terminal, and a screenshot of the running app with at least 3 todos, one completed.
