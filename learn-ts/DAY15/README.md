# DAY15: keyof, typeof, and Indexed Access Types

---

## "keyof is a key ring"

You have a key ring with several keys: house key, car key, office key, mailbox key. Each key opens a specific lock. The key ring holds the *names* of the keys, not the locks themselves.

`keyof` gives you the keys (property names) of a type:

```typescript
interface Person {
  name: string;
  age: number;
  email: string;
}

type PersonKeys = keyof Person;
// equivalent to: "name" | "age" | "email"
```

`PersonKeys` is a union of the literal strings `"name"`, `"age"`, `"email"`. You can use this to restrict a parameter to only valid property names:

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const person: Person = { name: "Alice", age: 30, email: "alice@example.com" };
console.log(getProperty(person, "name")); // "Alice" — type: string
console.log(getProperty(person, "age"));  // 30 — type: number
console.log(getProperty(person, "salary")); // ❌ Argument of type '"salary"' is not assignable to parameter of type 'keyof Person'
```

---

## typeof — capture a value's type

You already know `typeof` for runtime type checking. At the **type level**, `typeof` captures the TypeScript type of a value:

```typescript
const config = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
  retries: 3,
};

type ConfigType = typeof config;
// { apiUrl: string; timeout: number; retries: number; }

function updateConfig(partial: Partial<ConfigType>): void {
  Object.assign(config, partial);
}
```

This is incredibly useful when you want to derive a type from an existing value — especially configuration objects, constants, or third-party library results.

---

## Indexed access types — looking up a property type

You can look up the type of a specific property using bracket notation at the type level:

```typescript
interface Person {
  name: string;
  age: number;
  address: {
    street: string;
    city: string;
  };
}

type NameType = Person["name"];    // string
type AgeType = Person["age"];      // number
type AddressType = Person["address"]; // { street: string; city: string }

type NameOrAge = Person["name" | "age"]; // string | number

type AllValues = Person[keyof Person]; // string | number | { street: string; city: string }
```

---

## Combining them for type-safe getters

```typescript
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

function setValue<T, K extends keyof T>(obj: T, key: K, value: T[K]): void {
  obj[key] = value;
}

const user = { id: 1, name: "Bob", roles: ["admin"] };
setValue(user, "name", "Robert"); // ✅
setValue(user, "name", 42);       // ❌ Type 'number' not assignable to type 'string'
setValue(user, "id", "abc");      // ❌ Type 'string' not assignable to type 'number'
```

---

## Practical use: event system

```typescript
interface Events {
  click: { x: number; y: number };
  keypress: { key: string };
  focus: { element: HTMLElement };
}

function emit<K extends keyof Events>(event: K, payload: Events[K]): void {
  console.log(`Event: ${event}`, payload);
}

emit("click", { x: 10, y: 20 }); // ✅
emit("keypress", { key: "Enter" }); // ✅
emit("click", { key: "Enter" }); // ❌ Argument of type '{ key: string; }' is not assignable to parameter of type '{ x: number; y: number; }'
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| keyof | `type K = keyof T` — union of property names |
| typeof (type level) | `type T = typeof value` — captures the type |
| Indexed access | `T[K]` — type of property at key K |
| Generic key restriction | `K extends keyof T` |
| All values type | `T[keyof T]` — union of all property types |

Your turn. Open DAY15/DRILL.md.
