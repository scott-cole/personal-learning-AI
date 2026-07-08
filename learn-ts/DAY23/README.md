# DAY23: Express with TypeScript

---

## "Express is a mail sorting room"

A mail sorting room receives thousands of letters. Each letter arrives, gets stamped, sorted by ZIP code, and sent to the right bin. Some bins handle local delivery, some handle international, some handle packages.

Express is a mail sorting room for HTTP requests. Each request arrives at the server, gets matched to a **route** (handler), processed, and sent back as a response.

```typescript
import express, { Request, Response } from "express";

const app = express();
const port = 3000;

app.get("/", (req: Request, res: Response) => {
  res.send("Hello, world!");
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

---

## Setting up

```bash
npm install express
npm install -D @types/express @types/node tsx typescript
```

Express is JavaScript, so you need `@types/express` for TypeScript types.

---

## Route handlers

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

const users: User[] = [
  { id: 1, name: "Alice", email: "alice@example.com" },
  { id: 2, name: "Bob", email: "bob@example.com" },
];

app.get("/users", (req: Request, res: Response) => {
  res.json(users);
});

app.get("/users/:id", (req: Request<{ id: string }>, res: Response) => {
  const id = parseInt(req.params.id, 10);
  const user = users.find((u) => u.id === id);
  if (user) {
    res.json(user);
  } else {
    res.status(404).json({ error: "User not found" });
  }
});
```

`Request<{ id: string }>` types the `req.params` object, giving you typed access to `req.params.id`.

---

## Request body with typing

```typescript
app.use(express.json()); // middleware to parse JSON bodies

app.post("/users", (req: Request<{}, {}, { name: string; email: string }>, res: Response) => {
  const { name, email } = req.body;
  const newUser: User = {
    id: users.length + 1,
    name,
    email,
  };
  users.push(newUser);
  res.status(201).json(newUser);
});
```

The three type parameters are: `Params`, `ResBody`, `ReqBody`. You can skip unneeded ones with `{}`.

---

## Router — organizing routes

```typescript
// routes/users.ts
import { Router, Request, Response } from "express";

const router = Router();

router.get("/", (req: Request, res: Response) => {
  res.json(users);
});

router.get("/:id", (req: Request<{ id: string }>, res: Response) => {
  // ...
});

export default router;

// index.ts
import userRoutes from "./routes/users.js";
app.use("/users", userRoutes);
```

---

## Middleware

```typescript
import { Request, Response, NextFunction } from "express";

function logger(req: Request, res: Response, next: NextFunction): void {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next();
}

app.use(logger);
```

Middleware functions run before your route handlers. They can modify the request, log, authenticate, etc.

---

## Error handling middleware

```typescript
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack);
  res.status(500).json({ error: "Something went wrong!" });
});
```

This catches errors thrown in any route handler.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Basic route | `app.get("/path", handler)` |
| Typed params | `Request<{ id: string }>` |
| Typed body | `Request<{}, {}, PayloadType>` |
| JSON response | `res.json(data)` |
| Status code | `res.status(404).json({})` |
| Router | `const router = Router(); app.use("/path", router)` |
| Middleware | `app.use((req, res, next) => next())` |

Your turn. Open DAY23/DRILL.md.
