# DAY16: Mapped Types

---

## "Mapped types are a stamping machine"

A factory has a stamping machine. You feed in a metal sheet, and the machine stamps out hundreds of identical shapes — each one a copy with the same pattern.

Mapped types are a stamping machine for types. You feed in a type, and it stamps out a new type by transforming each property:

```typescript
interface Person {
  name: string;
  age: number;
  email: string;
}

type ReadonlyPerson = {
  readonly [K in keyof Person]: Person[K];
};

// equivalent to:
// { readonly name: string; readonly age: number; readonly email: string; }
```

The `[K in keyof Person]` iterates over each key. `Person[K]` gets the value type. `readonly` is applied to each.

---

## The syntax

```typescript
type Mapped<T> = {
  [K in keyof T]: T[K];
};
```

`[K in keyof T]` is like a `for...in` loop at the type level:
- `K` is the loop variable (each key)
- `keyof T` is the set of keys to iterate over
- `T[K]` is the value type for that key

---

## Common mapped type patterns

```typescript
// Make all properties optional
type Partial<T> = {
  [K in keyof T]?: T[K];
};

// Make all properties readonly
type Readonly<T> = {
  readonly [K in keyof T]: T[K];
};

// Make all properties required (remove optional)
type Required<T> = {
  [K in keyof T]-?: T[K];
};

// Make all properties mutable (remove readonly)
type Mutable<T> = {
  -readonly [K in keyof T]: T[K];
};
```

The `-` prefix removes `readonly` or `?`. `+` adds them (default behavior).

---

## Mapping with type transformation

You can transform the values:

```typescript
type StringifyValues<T> = {
  [K in keyof T]: string;
};

type Nullify<T> = {
  [K in keyof T]: T[K] | null;
};

type Wrapper<T> = {
  [K in keyof T]: { value: T[K] };
};

interface Person { name: string; age: number; }
// Wrapper<Person> → { name: { value: string }; age: { value: number } }
```

---

## Filtering properties by value type

Using `as` to remap keys (TypeScript 4.1+):

```typescript
type StringKeys<T> = {
  [K in keyof T as T[K] extends string ? K : never]: T[K];
};

interface Mixed {
  name: string;
  age: number;
  email: string;
}

// StringKeys<Mixed> → { name: string; email: string; }
```

The `as` clause lets you filter or rename keys during mapping.

---

## Prefixing keys

```typescript
type WithPrefix<T, P extends string> = {
  [K in keyof T as `${P}_${K & string}`]: T[K];
};

interface Person { name: string; age: number; }
// WithPrefix<Person, "user">
// → { user_name: string; user_age: number; }
```

This uses template literal types to transform key names.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Basic mapped type | `{ [K in keyof T]: T[K] }` |
| Add modifier | `{ readonly [K in keyof T]: T[K] }` |
| Remove modifier | `{ -readonly [K in keyof T]: T[K] }` |
| Key remapping | `{ [K in keyof T as NewKey]: T[K] }` |
| Value transform | `{ [K in keyof T]: string }` |

Your turn. Open DAY16/DRILL.md.
