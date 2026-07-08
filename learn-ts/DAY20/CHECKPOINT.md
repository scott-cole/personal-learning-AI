# DAY20: Checkpoint

**Closed book.** No looking back.

1. What does `"strict": true` enable (at least 3 sub-checks)?

2. What is the purpose of `"target": "ES2022"`?

3. What does `"sourceMap": true` do?

4. Why might you set `"declaration": true`?

5. What's the difference between `include` and `exclude` in tsconfig.json?

---

<details>
<summary>Answers</summary>

1. `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`, `strictPropertyInitialization`, `noUncheckedIndexedAccess` (any three).
2. It tells TypeScript to emit JavaScript compatible with ES2022 (modern class fields, optional chaining, etc.).
3. It generates `.js.map` files that let debuggers map compiled JS back to original TypeScript source.
4. To generate `.d.ts` declaration files, which are needed when publishing a library so consumers get type information.
5. `include` specifies which files to compile. `exclude` specifies which files to skip (typically `node_modules`, `dist`).
</details>
