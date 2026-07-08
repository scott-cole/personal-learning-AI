# DAY19: Checkpoint

**Closed book.** No looking back.

1. What is a `.d.ts` file used for?

2. What does the `declare` keyword do in TypeScript?

3. How would you add TypeScript types to a plain JavaScript npm package?

4. What is a module augmentation?

5. What does `npm install @types/express --save-dev` do?

---

<details>
<summary>Answers</summary>

1. It describes the types of JavaScript code without providing implementation. It's a "type-only" file.
2. It tells TypeScript that something exists in the ambient scope (global or module) without needing to provide an implementation.
3. Either install the `@types/package-name` package, or write a local `.d.ts` file with a `declare module "package-name" { ... }` block.
4. Adding new declarations to an existing module's types without changing the original module, using `declare module "module-name" { ... }`.
5. It installs the TypeScript type definitions for Express, so you get type checking and autocomplete when using Express.
</details>
