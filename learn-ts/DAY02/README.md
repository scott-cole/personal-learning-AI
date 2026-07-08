# DAY02: Functions, Parameters, Return Types, Arrow Functions

---

## "A function is a blender"

You have a countertop blender. You put ingredients in, press a button, and get a smoothie out.

A function is the same thing:

```
ingredients (inputs) → [ BLENDER ] → smoothie (output)
```

```typescript
function greet(name: string): string {
  return "Hello, " + name;
}

let message = greet("Scott");
console.log(message); // "Hello, Scott"
```

- `name: string` is the **parameter** — what goes in (the fruit)
- `: string` after the parentheses is the **return type** — what comes out (the smoothie)
- `return` hands the result back

You can't put a wrench in a blender. TypeScript ensures you pass the right type:

```typescript
greet(42); // ❌ Argument of type 'number' is not assignable to parameter of type 'string'
```

---

## Parameters are ingredients, return is the dish

```typescript
function add(a: number, b: number): number {
  return a + b;
}

console.log(add(3, 4)); // 7
```

You can have multiple parameters. Each one needs a type annotation. The return type is optional — TypeScript infers it — but explicit returns are better documentation.

```typescript
function fullName(first: string, last: string): string {
  return `${first} ${last}`;
}
```

---

## Arrow functions — the concise cousin

Arrow functions are shorter and behave differently with `this` (we'll cover that later).

```typescript
// Regular function
function double(x: number): number {
  return x * 2;
}

// Arrow function (same thing)
const double = (x: number): number => {
  return x * 2;
};

// Even shorter — implicit return (no curly braces = auto return)
const double = (x: number): number => x * 2;
```

When there's no `{}` after the arrow, the expression is **implicitly returned**.

```typescript
const square = (n: number): number => n * n;
console.log(square(5)); // 25
```

---

## Optional parameters

Sometimes an ingredient is optional. Use `?`:

```typescript
function greet(name: string, greeting?: string): string {
  if (greeting) {
    return `${greeting}, ${name}!`;
  }
  return `Hello, ${name}!`;
}

console.log(greet("Scott"));          // "Hello, Scott!"
console.log(greet("Scott", "Howdy")); // "Howdy, Scott!"
```

Optional parameters must come **after** required ones.

---

## Default parameters

Even cleaner — give it a fallback value:

```typescript
function greet(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}!`;
}

console.log(greet("Scott"));          // "Hello, Scott!"
console.log(greet("Scott", "Howdy")); // "Howdy, Scott!"
```

---

## void return — when there's no output

Some functions don't return anything. They just do something (side effect):

```typescript
function logMessage(msg: string): void {
  console.log(msg);
  // no return statement
}
```

`void` means "this function doesn't return a meaningful value."

---

## Summary

| Concept | Syntax |
|---------|--------|
| Regular function | `function add(a: number, b: number): number { return a + b; }` |
| Arrow function | `const add = (a: number, b: number): number => a + b;` |
| Optional param | `function greet(name?: string)` |
| Default param | `function greet(name: string = "World")` |
| Void return | `function log(msg: string): void` |
| Call a function | `add(3, 4)` |

Your turn. Open DAY02/DRILL.md.
