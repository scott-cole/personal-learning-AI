# DAY10: Checkpoint

**Closed book.** No looking back.

1. What's the difference between `export default` and a named export?

2. How do you import a default export differently from a named export?

3. What does `import * as Stuff from "./module"` give you?

4. Why use `import type { Foo }` instead of `import { Foo }`?

5. What does a barrel file (index.ts) typically do?

---

<details>
<summary>Answers</summary>

1. A module can have only one default export. It can have many named exports. Default exports are imported without braces.
2. Default: `import Foo from "./bar"`. Named: `import { Foo } from "./bar"`.
3. A namespace object containing all named exports as properties (e.g., `Stuff.foo`, `Stuff.bar`).
4. It tells TypeScript and bundlers that the import is only for type checking and can be safely elided in the output.
5. Re-exports all symbols from a folder's modules, providing a single entry point: `export { Foo } from "./foo"; export { Bar } from "./bar";`.
</details>
