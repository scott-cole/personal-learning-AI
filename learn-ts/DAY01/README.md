# DAY01: Variables, Types, and console.log

---

## "Variables are labelled boxes on a shelf"

Walk into a garage. On a shelf sit boxes, each with a label and a purpose.

- A box labelled `"books"` holds books. You wouldn't shove a greasy wrench in there.
- A box labelled `"tools"` holds tools. You wouldn't stuff a novel in it.

Variables work the same way. You declare a name, give it a type (what kind of thing fits), and store a value.

```typescript
let name: string = "Scott";
let age: number = 30;
let isStudent: boolean = false;
```

The label is `name`. The type is `string` — only text goes in. The value is `"Scott"`.

**TypeScript adds the type annotations that JavaScript leaves out.** In plain JS, you'd write `let name = "Scott"` — and hope nobody reassigns it to a number later. TypeScript prevents that chaos.

```typescript
let name: string = "Scott";
name = 42; // ❌ Type 'number' is not assignable to type 'string'
```

The compiler catches this before your code ever runs. That's the whole point.

---

## The basic types

| Type | What it holds | Examples |
|------|--------------|----------|
| `string` | Text | `"hello"`, `"42"`, backtick strings |
| `number` | All numbers (int & decimal) | `42`, `3.14`, `-5`, `1e6` |
| `boolean` | True/false | `true`, `false` |
| `null` | Intentional nothing | `null` |
| `undefined` | Unintentional nothing | `undefined` |

**Production note:** Unlike Go, TypeScript has a single `number` type for both integers and decimals. `0.1 + 0.2` still gives `0.30000000000000004` — store money in cents as an integer.

---

## `let` vs `const`

```typescript
let age: number = 30;    // can change later
age = 31;                 // ✅ fine

const name: string = "Scott"; // cannot change
name = "Steve";           // ❌ Error: cannot reassign a const
```

Use `const` by default. Use `let` only when you know the value will change. This signals intent and prevents accidental reassignment.

---

## console.log — your debugging best friend

```typescript
console.log("Hello, world!");
console.log(name);          // prints the value of name
console.log("Age:", age);   // prints: Age: 30
```

Think of `console.log` as a flashlight in a dark room. You shine it on variables to see what they hold.

```typescript
let x: number = 10;
let y: number = 20;
console.log("x + y =", x + y); // prints: x + y = 30
```

---

## Type inference

You don't always need to write the type. TypeScript can figure it out:

```typescript
let name = "Scott";    // TypeScript knows this is a string
let age = 30;          // knows this is a number
let isStudent = false; // knows this is a boolean
```

This is called **type inference**. The compiler looks at the value and infers the type. Use inference for simple cases, explicit annotations for function signatures and complex types.

---

## Running TypeScript

Save a file as `index.ts`, then run with `tsx` (TypeScript execute):

```bash
npx tsx index.ts
```

Or compile first and run the JavaScript:

```bash
npx tsc index.ts && node index.js
```

For this course we'll use `npx tsx` — it's the quickest way to run TypeScript files directly.

---

## Senior mindset: "Let the compiler be your pair programmer"

Every red squiggle in your editor is a bug that didn't reach production. TypeScript's compiler is not your enemy — it's a strict senior dev reviewing every line before it ships.

Write 3 lines. Run them. See it work. Add 3 more. Run again. This cadence compounds your learning.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Declare with type | `let name: string = "Scott"` |
| Declare with inference | `let name = "Scott"` |
| Constant | `const pi = 3.14` |
| Print to console | `console.log("hello")` |
| Run with tsx | `npx tsx index.ts` |

Your turn. Open DAY01/DRILL.md.
