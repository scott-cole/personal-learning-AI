# DAY23: Drill — "User CRUD API with Express"

Build a RESTful API for users in `DAY23/`.

## File structure

```
DAY23/
  package.json
  tsconfig.json
  src/
    index.ts          — entry point
    routes/
      users.ts        — user CRUD routes
    types.ts          — User interface
    data.ts           — in-memory user store
```

## Requirements

### types.ts
```typescript
export interface User {
  id: number;
  name: string;
  email: string;
  createdAt: string;
}
```

### data.ts
```typescript
import { User } from "./types.js";

const users: User[] = [
  { id: 1, name: "Alice", email: "alice@example.com", createdAt: "2024-01-15" },
  { id: 2, name: "Bob", email: "bob@example.com", createdAt: "2024-02-20" },
];

export function getUsers(): User[];
export function getUserById(id: number): User | undefined;
export function createUser(name: string, email: string): User;
export function updateUser(id: number, changes: Partial<Omit<User, "id" | "createdAt">>): User | undefined;
export function deleteUser(id: number): boolean;
```

### routes/users.ts
```typescript
// GET    /users        → list all
// GET    /users/:id    → get by id (404 if not found)
// POST   /users        → create (validate name & email in body)
// PUT    /users/:id    → update (merge changes)
// DELETE /users/:id    → delete (return 204 No Content)
```

### index.ts
```typescript
import express from "express";
import userRoutes from "./routes/users.js";

const app = express();
app.use(express.json());
app.use("/users", userRoutes);

app.listen(3000, () => console.log("Server on http://localhost:3000"));
```

## Testing with curl

```bash
curl http://localhost:3000/users
curl http://localhost:3000/users/1
curl -X POST http://localhost:3000/users -H "Content-Type: application/json" -d '{"name":"Charlie","email":"c@d.com"}'
curl -X PUT http://localhost:3000/users/1 -H "Content-Type: application/json" -d '{"name":"Alice Smith"}'
curl -X DELETE http://localhost:3000/users/2
```

## Hints

<details>
<summary>Click for hints</summary>

- `req.params.id` is a string — parse with `parseInt(id, 10)`
- `res.status(204).send()` for delete (no content)
- Use `Omit<User, "id" | "createdAt">` for update payloads
- Add validation: name must be non-empty, email must contain `@`
</details>

## Bonus

Add validation middleware that checks for missing/empty fields on POST. Add a search query: `GET /users?search=Ali` that filters by name or email.

## When you're done

Show me all your files and a curl session demonstrating each endpoint.
