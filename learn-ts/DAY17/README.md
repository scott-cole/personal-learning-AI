# DAY17: Conditional Types

---

## "Conditional types are a coin sorter"

A coin sorter has slots of different sizes. Drop a handful of coins in. Quarters go in one slot, dimes in another, pennies in another. The machine inspects each coin — if it's a certain size, it goes to a certain slot.

Conditional types inspect a type and pick a different type based on a condition:

```typescript
type IsString<T> = T extends string ? "yes" : "no";

type A = IsString<"hello">;  // "yes"
type B = IsString<42>;       // "no"
type C = IsString<boolean>;  // "no"
```

The syntax: `T extends U ? TrueBranch : FalseBranch`

This is a ternary operator for types. If `T` is assignable to `U`, pick the true branch; otherwise, pick the false branch.

---

## Why conditional types matter

They unlock dynamic type transformations based on input types:

```typescript
type ElementType<T> = T extends (infer U)[] ? U : T;

type A = ElementType<string[]>;  // string
type B = ElementType<number[]>;  // number
type C = ElementType<boolean>;   // boolean (not an array, returns itself)
```

`infer` is the magic keyword — it says "figure out what `U` is from the structure."

---

## infer — pattern matching for types

```typescript
// Extract the return type of a function
type ReturnType<T> = T extends (...args: unknown[]) => infer R ? R : never;

type Fn = (x: number) => string;
type R = ReturnType<Fn>; // string

// Extract the parameter types
type Parameters<T> = T extends (...args: infer P) => unknown ? P : never;

type P = Parameters<Fn>; // [x: number]

// Extract Promise value
type Unwrap<T> = T extends Promise<infer U> ? U : T;

type Wrapped = Unwrap<Promise<string>>; // string
type NotWrapped = Unwrap<number>;       // number
```

---

## Distributive conditional types

When you apply a conditional type to a union, it **distributes** — applies to each member independently:

```typescript
type ToArray<T> = T extends unknown ? T[] : never;

type Result = ToArray<string | number>;
// string[] | number[] — NOT (string | number)[]
```

This happens automatically when `T` is a bare type parameter. To prevent distribution, wrap both sides:

```typescript
type ToArrayNonDist<T> = [T] extends [unknown] ? T[] : never;
type Result2 = ToArrayNonDist<string | number>;
// (string | number)[] — single array of union
```

---

## Filtering union members

```typescript
type ExtractStrings<T> = T extends string ? T : never;

type Mixed = "hello" | 42 | true | "world";
type StringsOnly = ExtractStrings<Mixed>; // "hello" | "world"
```

This is how the built-in `Extract<T, U>` and `Exclude<T, U>` work:

```typescript
type Extract<T, U> = T extends U ? T : never;
type Exclude<T, U> = T extends U ? never : T;
```

---

## Practical: function overload types

```typescript
type AsyncResult<T> = T extends (...args: infer A) => infer R
  ? (...args: A) => Promise<R>
  : T;

function syncAdd(a: number, b: number): number {
  return a + b;
}

type AsyncAdd = AsyncResult<typeof syncAdd>;
// (a: number, b: number) => Promise<number>
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Basic conditional | `T extends U ? X : Y` |
| infer | `T extends (infer U)[] ? U : T` |
| Distribution | Automatic on bare `T` in `T extends U` |
| Prevent distribution | `[T] extends [U]` |
| Extract from union | `T extends U ? T : never` |

Your turn. Open DAY17/DRILL.md.
