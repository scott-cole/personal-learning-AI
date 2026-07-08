# DAY30: Final Project — Full-Stack Todo with React + Express + TypeScript

---

## "Full-stack is a restaurant"

A restaurant has a front of house (waiters, menu, dining area) and a back of house (kitchen, chefs, ingredients). The front takes orders and serves food. The back cooks. They communicate through a pass-through window — the API.

A full-stack app is the same. The frontend (React) is the dining room — it displays data and captures user input. The backend (Express) is the kitchen — it processes data and talks to the database. The API is the pass-through window.

---

## Architecture

```
client/              — React + TypeScript (Vite)
  src/
    App.tsx
    components/
      TodoList.tsx
      TodoInput.tsx
    hooks/
      useTodos.ts     — fetch, add, toggle, delete
    types.ts

server/              — Express + TypeScript
  src/
    index.ts
    routes/
      todos.ts
    types.ts
    schemas.ts
```

---

## API specification

```
GET    /api/todos     → Todo[]
POST   /api/todos     → Create todo { title: string }
PUT    /api/todos/:id → Update todo { title?, completed? }
DELETE /api/todos/:id → Delete todo → 204
```

---

## The Todo type (shared)

```typescript
interface Todo {
  id: number;
  title: string;
  completed: boolean;
  createdAt: string;
}
```

---

## Server setup

```typescript
// server/src/index.ts
import express from "express";
import cors from "cors";
import todoRoutes from "./routes/todos.js";

const app = express();
app.use(cors());
app.use(express.json());
app.use("/api/todos", todoRoutes);

app.listen(3001, () => console.log("Server on :3001"));
```

---

## Client setup

```typescript
// client/src/hooks/useTodos.ts
function useTodos() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("http://localhost:3001/api/todos")
      .then((res) => res.json())
      .then((data) => {
        setTodos(data);
        setLoading(false);
      });
  }, []);

  const addTodo = async (title: string) => {
    const res = await fetch("http://localhost:3001/api/todos", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ title }),
    });
    const todo = await res.json();
    setTodos((prev) => [...prev, todo]);
  };

  const toggleTodo = async (id: number) => {
    const todo = todos.find((t) => t.id === id);
    if (!todo) return;
    const res = await fetch(`http://localhost:3001/api/todos/${id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ completed: !todo.completed }),
    });
    const updated = await res.json();
    setTodos((prev) => prev.map((t) => (t.id === id ? updated : t)));
  };

  const deleteTodo = async (id: number) => {
    await fetch(`http://localhost:3001/api/todos/${id}`, { method: "DELETE" });
    setTodos((prev) => prev.filter((t) => t.id !== id));
  };

  return { todos, loading, addTodo, toggleTodo, deleteTodo };
}
```

---

## What the 30 days built

| Week | Focus | You can now |
|------|-------|-------------|
| 1 | Fundamentals | Variables, functions, objects, arrays, unions, classes |
| 2 | Intermediate | Generics, enums, modules, async, DOM, API client |
| 3 | Advanced | keyof, mapped types, conditional types, utility types, declaration files |
| 4 | Production | Node/Express, Zod, testing, React, context, full-stack apps |

Your turn. Open DAY30/DRILL.md.
