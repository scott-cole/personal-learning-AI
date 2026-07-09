# DAY07: Drill — "CLI Todo Manager"

Build a complete CLI todo application in `DAY07/`.

## File structure

```
DAY07/
  index.ts    — entry point, parses args, calls TodoManager
  todo.ts     — TodoManager class
  types.ts    — interfaces and types
```

## Requirements

### types.ts
```typescript
export interface TodoItem {
  id: number;
  title: string;
  completed: boolean;
  createdAt: Date;
}

export type Action = "add" | "list" | "done" | "remove" | "help";
```

### todo.ts
- `class TodoManager` with a private `items: TodoItem[]` array
- `add(title: string): TodoItem` — creates item, adds to list, returns it
- `list(): TodoItem[]` — returns all items
- `markDone(id: number): TodoItem | undefined` — finds by id, sets completed, returns it or undefined
- `remove(id: number): boolean` — removes by id, returns true if found

### index.ts
- Reads `process.argv.slice(2)` to get action and argument
- `add <title>` — calls add, prints confirmation
- `list` — prints numbered list with [ ] or [✓]
- `done <id>` — calls markDone, prints confirmation or "not found"
- `remove <id>` — calls remove, prints confirmation or "not found"
- `help` — prints available commands
- Unknown action — prints error and shows help

## State persistence

Data lives in memory only (no file I/O). Restarting resets the list. That's fine — we'll add persistence later.

## Hints

<details>
<summary>Click for hints</summary>

- `process.argv` gives `['node', 'path/to/file', 'add', 'Buy groceries']` — slice off the first 2
- Use `parseInt(args[1], 10)` to convert the id string to a number
- For `list`, iterate with a `for` loop or `.forEach()` and print each item
- Use `(item.completed ? "✓" : " ")` for the checkbox display
- Export/import with `export class` and `import { ... } from "./todo"`
</details>

## Expected output

```
$ npx tsx index.ts help
Commands:
  add <title>    Add a new todo
  list           Show all todos
  done <id>      Mark a todo as done
  remove <id>    Remove a todo
  help           Show this help

$ npx tsx index.ts list
Your todo list is empty.

$ npx tsx index.ts add "Finish this project"
→ Added: Finish this project (id: 1)
```

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY07
npx tsx index.ts add "Test todo"
npx tsx index.ts list
```

## When you're done

Show me all three files. I'll review them together.
