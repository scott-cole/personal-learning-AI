# DAY08: Generics Basics

---

## "Generics are a universal adapter"

You travel to a different country. Your phone charger has a US plug, but the wall socket is European. You need a universal adapter — one that works with any plug shape and any socket type.

Generics are the type-system equivalent. They let you write a function, class, or interface that works with **any type** while still enforcing type safety.

```typescript
function identity<T>(value: T): T {
  return value;
}

const num = identity(42);       // type: number
const str = identity("hello");  // type: string
const bool = identity(true);    // type: boolean
```

`<T>` is a **type parameter** — it's a placeholder. When you call the function, TypeScript fills in the real type. The `T` is like a variable for a type.

---

## Why not just `any`?

```typescript
function identityBad(value: any): any {
  return value;
}

const x = identityBad(42);
x.toUpperCase(); // ❌ No error at compile time, crashes at runtime
```

`any` turns off type checking. Generics preserve the type information:

```typescript
function identity<T>(value: T): T {
  return value;
}

const x = identity(42);
x.toUpperCase(); // ❌ Property 'toUpperCase' does not exist on type 'number'
```

---

## Generic arrays

You've already used generics without knowing it. `number[]` is syntactic sugar for `Array<number>`:

```typescript
const nums: Array<number> = [1, 2, 3];
const names: Array<string> = ["Alice", "Bob"];

function first<T>(arr: T[]): T | undefined {
  return arr[0];
}

console.log(first([1, 2, 3]));     // 1 (type: number | undefined)
console.log(first(["a", "b"]));    // "a" (type: string | undefined)
```

---

## Generic interfaces

```typescript
interface Box<T> {
  value: T;
  label: string;
}

const stringBox: Box<string> = { value: "hello", label: "greeting" };
const numberBox: Box<number> = { value: 42, label: "answer" };
```

---

## Generic constraints

Sometimes you need to restrict what types `T` can be. Use `extends`:

```typescript
interface HasLength {
  length: number;
}

function logLength<T extends HasLength>(item: T): void {
  console.log(`Length: ${item.length}`);
}

logLength("hello");        // Length: 5 (string has .length)
logLength([1, 2, 3]);      // Length: 3 (array has .length)
logLength(42);             // ❌ Type 'number' does not have a 'length' property
```

`T extends HasLength` means "T must have at least the shape of HasLength."

---

## Multiple type parameters

```typescript
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}

const p = pair("hello", 42); // type: [string, number]
```

---

## Generic classes

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }
}

const stack = new Stack<number>();
stack.push(1);
stack.push(2);
console.log(stack.pop()); // 2
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Generic function | `function foo<T>(x: T): T` |
| Generic interface | `interface Box<T> { value: T }` |
| Generic constraint | `function foo<T extends HasLength>` |
| Multiple params | `function pair<T, U>(a: T, b: U): [T, U]` |
| Generic class | `class Stack<T> { ... }` |
| Array generic | `Array<number>` same as `number[]` |

Your turn. Open DAY08/DRILL.md.
