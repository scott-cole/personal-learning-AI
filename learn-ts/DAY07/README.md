# DAY07: CLI Todo Manager (Project)

---

## "A project is a kitchen remodel"

You've learned the individual tools: a hammer (variables), a saw (functions), a level (interfaces), a measuring tape (arrays). Now it's time to remodel the kitchen — build something real that combines everything.

A CLI todo manager is the "hello world" of projects. It ties together everything from Week 1 into a program you'd actually use.

---

## What we're building

A command-line todo list that runs in the terminal:

```
$ npx tsx index.ts add "Buy groceries"
→ Added: Buy groceries

$ npx tsx index.ts add "Learn TypeScript"
→ Added: Learn TypeScript

$ npx tsx index.ts list
1. [ ] Buy groceries
2. [ ] Learn TypeScript

$ npx tsx index.ts done 1
→ Marked #1 as done

$ npx tsx index.ts list
1. [✓] Buy groceries
2. [ ] Learn TypeScript
```

---

## Architecture

```
index.ts           — entry point, reads command-line args
todo.ts            — TodoManager class (the brain)
types.ts           — TodoItem interface, action types
```

---

## Key types

```typescript
// types.ts
export interface TodoItem {
  id: number;
  title: string;
  completed: boolean;
  createdAt: Date;
}

export type Action = "add" | "list" | "done" | "remove" | "help";
```

---

## The TodoManager class

```typescript
// todo.ts
import { TodoItem, Action } from "./types";

export class TodoManager {
  private items: TodoItem[] = [];
  private nextId = 1;

  add(title: string): TodoItem { /* ... */ }
  list(): TodoItem[] { /* ... */ }
  markDone(id: number): TodoItem | undefined { /* ... */ }
  remove(id: number): boolean { /* ... */ }
}
```

---

## Reading command-line args

```typescript
// index.ts
const args = process.argv.slice(2); // ['add', 'Buy groceries']
const action = args[0] as Action;
const argument = args.slice(1).join(" ");
```

---

## What Week 1 taught you for this

| Day | Skill | Used in project |
|-----|-------|----------------|
| DAY01 | Variables, console.log | Every line of output |
| DAY02 | Functions | Each TodoManager method |
| DAY03 | Interfaces, objects | TodoItem, return types |
| DAY04 | Arrays, array methods | Storing items, finding by id |
| DAY05 | Union types, type aliases | Action type, return types |
| DAY06 | Classes, private fields | TodoManager class |

Your turn. Open DAY07/DRILL.md.
