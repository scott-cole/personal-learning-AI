# DAY07 Checkpoint

## Questions

1. What does `if __name__ == "__main__"` do?
2. Why do we use `try/except` when loading the JSON file?
3. What's the purpose of the auto-incrementing ID?
4. What happens if the user enters "abc" when asked for a task ID?
5. Which line saves changes permanently to disk?

<details>
<summary>Answers</summary>

1. It ensures `main()` only runs when the script is executed directly, not when imported as a module.
2. Because `todos.json` may not exist on the first run — `FileNotFoundError` is caught and we return an empty list.
3. It guarantees each task has a unique identifier so we can mark or delete specific tasks, even if multiple tasks have the same text.
4. `int("abc")` raises `ValueError`, which is caught and prints `"Invalid ID."` instead of crashing.
5. `json.dump(tasks, f, indent=2)` inside `save_tasks()` writes the data to the file.

</details>
