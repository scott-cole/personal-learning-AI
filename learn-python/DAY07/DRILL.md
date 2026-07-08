# DAY07 Drill — Build the Todo Manager

## Task

Write a complete CLI Todo Manager (`todo.py`) following the spec in README.md.

### Requirements

- Menu loop with 5 options
- List tasks with `[✓]` or `[ ]` status indicators
- Add task with auto-incrementing ID
- Mark task as done
- Delete task by ID
- Save/load from `todos.json`
- Handle the case where `todos.json` doesn't exist yet
- Handle invalid input gracefully (no crashes)

### Expected behaviour

```
===== TODO MANAGER =====
1. List tasks
2. Add task
3. Mark done
4. Delete task
5. Exit
Choice: 2
Task: Learn Python
Task added!

===== TODO MANAGER =====
1. List tasks
2. Add task
3. Mark done
4. Delete task
5. Exit
Choice: 1
1. [ ] Learn Python
```

### Hints

- Use `try/except` for both file operations and `int()` conversion
- `max((t["id"] for t in tasks), default=0) + 1` for new IDs
- Call `save_tasks(tasks)` after every modification
- File structure: `todos.json` sits next to your `todo.py`

### How to run

```bash
python3 todo.py
```

**Close the program with option 5**, then inspect `todos.json` to see your saved data.

## TODOs

- [ ] `load_tasks()` — read from `todos.json`, return empty list if missing
- [ ] `save_tasks(tasks)` — write to `todos.json` with `json.dump`
- [ ] `list_tasks(tasks)` — iterate and print with status symbol
- [ ] `add_task(tasks)` — get input, create dict, append, save
- [ ] `mark_done(tasks)` — find by ID, set `done=True`, save
- [ ] `delete_task(tasks)` — find by index, `pop()`, save
- [ ] `main()` — menu loop with `if/elif/else` and `break`
- [ ] `if __name__ == "__main__"` guard
