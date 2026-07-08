# DAY09: Enums, Type Narrowing, typeof/instanceof

---

## "Type narrowing is a metal detector"

You're on a beach with a metal detector. It beeps. Is it a coin? A bottle cap? A buried treasure? You dig and look — that's **narrowing** the possibilities.

TypeScript starts with a wide type like `string | number | boolean`. Through checks, you narrow it down to a specific type.

---

## Enums — named constants

An enum is a set of named constants. Think of it as a menu with labelled items:

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}

let dir: Direction = Direction.North;
console.log(dir); // 0 (defaults to 0, 1, 2, 3)
```

You can set custom values:

```typescript
enum StatusCode {
  OK = 200,
  NotFound = 404,
  InternalError = 500,
}

console.log(StatusCode.OK); // 200
console.log(StatusCode[404]); // "NotFound" (reverse mapping)
```

String enums are common:

```typescript
enum Color {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE",
}
```

---

## typeof — check the type at runtime

```typescript
function process(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase()); // ✅ narrowed to string
  } else {
    console.log(value.toFixed(2));    // ✅ narrowed to number
  }
}
```

`typeof` works with: `"string"`, `"number"`, `"boolean"`, `"symbol"`, `"bigint"`, `"undefined"`, `"object"`, `"function"`.

It does **not** work for custom types — `typeof` always returns `"object"` for arrays, null, and objects.

---

## instanceof — check the class

```typescript
class Dog {
  bark() { console.log("Woof!"); }
}

class Cat {
  meow() { console.log("Meow!"); }
}

function makeSound(animal: Dog | Cat): void {
  if (animal instanceof Dog) {
    animal.bark(); // ✅ narrowed
  } else {
    animal.meow(); // ✅ narrowed
  }
}
```

---

## Discriminated unions — narrowing with a tag

The most powerful narrowing pattern:

```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; sideLength: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2; // ✅ narrowed
    case "square":
      return shape.sideLength ** 2;       // ✅ narrowed
  }
}
```

Each branch of the `switch` narrows the type based on the `kind` discriminant.

---

## Truthiness narrowing

Simple but effective — check if a value exists:

```typescript
function printLength(s?: string): void {
  if (s) {
    console.log(s.length); // narrowed: s is string, not undefined
  } else {
    console.log("No string provided");
  }
}
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Enum | `enum Color { Red, Green }` |
| typeof narrowing | `if (typeof x === "string")` |
| instanceof narrowing | `if (x instanceof Dog)` |
| Discriminated union | `switch (x.kind) { case "circle": ... }` |
| Truthiness narrowing | `if (x) { ... }` |

Your turn. Open DAY09/DRILL.md.
