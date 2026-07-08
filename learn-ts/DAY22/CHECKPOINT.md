# DAY22: Checkpoint

**Closed book.** No looking back.

1. What's the main advantage of `tsx` over `ts-node`?

2. What does `"type": "module"` in package.json do?

3. What does `tsc --noEmit` do differently from plain `tsc`?

4. Why might you use esbuild instead of tsc for compilation?

5. What does `tsx watch src/index.ts` do?

---

<details>
<summary>Answers</summary>

1. `tsx` uses esbuild under the hood, making it significantly faster than `ts-node` (which uses TypeScript's compiler API).
2. It tells Node.js to use ES modules (ESM) — `import`/`export` syntax instead of `require()`.
3. It performs type checking but does NOT generate any output files. Use it for CI type-checking.
4. esbuild is much faster (10-100x). It's ideal for development builds. However, it doesn't type-check — you need a separate `tsc --noEmit` step.
5. It runs the file with tsx and automatically restarts whenever any source file changes.
</details>
