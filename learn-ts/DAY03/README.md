# DAY03: Objects and Interfaces

---

## "An interface is a blueprint"

Before you build a house, an architect draws a blueprint. It says: "There will be a front door here, three windows there, and a roof with this slope." The blueprint is not the house — it's the **plan** for the house.

An interface in TypeScript is the same thing. It describes the shape of an object without creating it:

```typescript
interface Person {
  name: string;
  age: number;
  isStudent: boolean;
}
```

This says: "Any object that claims to be a `Person` must have a `name` (string), an `age` (number), and an `isStudent` (boolean)."

```typescript
const scott: Person = {
  name: "Scott",
  age: 30,
  isStudent: false,
}; // ✅ matches the blueprint

const bad: Person = {
  name: "Scott",
}; // ❌ Property 'age' is missing
```

---

## Objects — the actual houses

An object literal is the actual house built from the blueprint:

```typescript
const person: Person = {
  name: "Alice",
  age: 25,
  isStudent: true,
};

console.log(person.name); // "Alice"
console.log(person["age"]); // 25 (bracket notation, less common)
```

You can also inline the type without a named interface:

```typescript
const person: { name: string; age: number } = {
  name: "Alice",
  age: 25,
};
```

But named interfaces are reusable. Always prefer them when an object shape appears more than once.

---

## Optional properties

Not every house has a garage. Mark optional properties with `?`:

```typescript
interface Car {
  make: string;
  model: string;
  year?: number; // optional
}

const car1: Car = { make: "Toyota", model: "Corolla" };        // ✅
const car2: Car = { make: "Honda", model: "Civic", year: 2020 }; // ✅
```

---

## readonly properties

Some things shouldn't change after construction — like a house's foundation:

```typescript
interface Config {
  readonly apiKey: string;
  readonly endpoint: string;
}

const config: Config = {
  apiKey: "abc-123",
  endpoint: "https://api.example.com",
};

config.apiKey = "new-key"; // ❌ Cannot assign to 'apiKey' because it is a read-only property
```

---

## Methods in interfaces

Interfaces can also describe what functions an object should have:

```typescript
 interface Calculator {
  add(a: number, b: number): number;
  subtract(a: number, b: number): number;
}

const basicCalc: Calculator = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
};
```

---

## Nested objects

Blueprints can reference other blueprints:

```typescript
interface Address {
  street: string;
  city: string;
}

interface Employee {
  name: string;
  address: Address; // nested interface
}

const emp: Employee = {
  name: "Bob",
  address: {
    street: "123 Main St",
    city: "Portland",
  },
};
```

---

## Type aliases vs interfaces

There's also `type` which can do similar things:

```typescript
type Person = {
  name: string;
  age: number;
};
```

The difference: `interface` can be extended (merged), `type` cannot. Use `interface` for object shapes, `type` for unions and aliases (which we'll see later).

---

## Summary

| Concept | Syntax |
|---------|--------|
| Interface | `interface Person { name: string }` |
| Object with type | `const p: Person = { name: "Alice" }` |
| Optional property | `nickname?: string` |
| Readonly property | `readonly id: string` |
| Nested interface | `interface Emp { address: Address }` |
| Type alias | `type Person = { name: string }` |

Your turn. Open DAY03/DRILL.md.
