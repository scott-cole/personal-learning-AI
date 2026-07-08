# DAY07: Checkpoint

**Closed book.** No looking back.

1. What does `process.argv` return in a Node.js TypeScript program?

2. Write a type that represents a discriminated union for `"add"`, `"list"`, `"done"`, `"remove"`, and `"help"` actions.

3. Why does `TodoManager` store `items` as `private`?

4. What does `.find()` return if no element matches the condition?

5. How would you make the todo list persist between runs (conceptually)?

---

<details>
<summary>Answers</summary>

1. An array of strings: `['node/path', '/script/path', 'arg1', 'arg2', ...]`. The first two entries are the runtime and script path.
2. `type Action = "add" | "list" | "done" | "remove" | "help";`
3. Encapsulation — external code shouldn't directly manipulate the items array. All access goes through methods.
4. `undefined`
5. Read/write a JSON file (`todos.json`) using `fs.readFileSync` and `fs.writeFileSync` on startup and after each mutation.
</details>
