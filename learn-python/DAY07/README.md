# DAY07 — Project: CLI Todo Manager

## Anecdote: Your first workshop build

So far you've learned to pick up individual tools — variables are labelled boxes, functions are blenders, strings are beads, lists are lockers, dicts are coat checks. Now it's time to *build something* with all of them together.

Think of this project as your first piece of furniture from the workshop: a simple wooden stool. It's not fancy, but it requires measuring (variables), cutting (functions), joining (data structures), and finishing (file I/O). Every master carpenter started with a stool.

Today you build a **CLI Todo Manager** — a program that runs in the terminal, keeps your tasks in a file, and never forgets what you need to do.

---

## What we're building

A command-line todo list that:

- Lists all tasks
- Adds a new task
- Marks a task as done
- Deletes a task
- Saves everything to a file so it persists between runs

We use everything from Days 1–6:
| Concept | Where it fits |
|---------|--------------|
| Variables | Task data, filenames, counters |
| Functions | Each command is a function |
| Strings | F-strings for output, user input |
| Lists | The task list in memory |
| Dicts | Each task is a dict with `id`, `task`, `done` |
| Conditionals | Which command to run |
| Loops | Display tasks, process commands |

---

## How the program works

When you run the script, it shows a menu:

```
===== TODO MANAGER =====
1. List tasks
2. Add task
3. Mark done
4. Delete task
5. Exit
=========================
Choice:
```

The user types a number. The program does the action, then shows the menu again until the user chooses Exit.

---

## File persistence

Tasks are stored in `todos.json` as a JSON array:

```json
[
    {"id": 1, "task": "Buy groceries", "done": false},
    {"id": 2, "task": "Finish Python project", "done": true}
]
```

We use the built-in `json` module:

```python
import json

def load_tasks():
    try:
        with open("todos.json", "r") as f:
            return json.load(f)
    except FileNotFoundError:
        return []

def save_tasks(tasks):
    with open("todos.json", "w") as f:
        json.dump(tasks, f, indent=2)
```

---

## Core functions

```python
def list_tasks(tasks):
    if not tasks:
        print("No tasks yet!")
        return
    for t in tasks:
        status = "✓" if t["done"] else " "
        print(f"{t['id']}. [{status}] {t['task']}")

def add_task(tasks):
    task_text = input("Task: ")
    new_id = max((t["id"] for t in tasks), default=0) + 1
    tasks.append({"id": new_id, "task": task_text, "done": False})
    save_tasks(tasks)
    print("Task added!")

def mark_done(tasks):
    list_tasks(tasks)
    try:
        task_id = int(input("Task ID to mark done: "))
        for t in tasks:
            if t["id"] == task_id:
                t["done"] = True
                save_tasks(tasks)
                print("Marked done!")
                return
        print("Task not found.")
    except ValueError:
        print("Invalid ID.")

def delete_task(tasks):
    list_tasks(tasks)
    try:
        task_id = int(input("Task ID to delete: "))
        for i, t in enumerate(tasks):
            if t["id"] == task_id:
                tasks.pop(i)
                save_tasks(tasks)
                print("Deleted!")
                return
        print("Task not found.")
    except ValueError:
        print("Invalid ID.")
```

---

## The main loop

```python
def main():
    tasks = load_tasks()
    while True:
        print("\n===== TODO MANAGER =====")
        print("1. List tasks")
        print("2. Add task")
        print("3. Mark done")
        print("4. Delete task")
        print("5. Exit")
        choice = input("Choice: ")

        if choice == "1":
            list_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            mark_done(tasks)
        elif choice == "4":
            delete_task(tasks)
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
```

The `if __name__ == "__main__"` guard ensures `main()` only runs when you execute the script directly, not when you import it.

---

## Running the project

```bash
python3 todo.py
```

---

## Summary

| Part | What you used |
|------|--------------|
| Menu loop | `while True`, `if/elif/else`, `break` |
| Task display | `for` loop, f-strings, conditionals |
| Data structure | `list` of `dict` with `id`, `task`, `done` |
| Persistence | `json` module, file I/O with `with` |
| Input | `input()` function |
| Entry point | `if __name__ == "__main__"` |
