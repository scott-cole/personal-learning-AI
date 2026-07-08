# DAY18: Utility Types (Partial, Pick, Omit, Readonly, Record)

---

## "Utility types are a Swiss Army knife"

A Swiss Army knife has a blade, scissors, a corkscrew, a toothpick, tweezers — a dozen tools in one compact package. You don't build each tool from scratch; you reach into the handle and pull out what you need.

TypeScript's built-in utility types are the same. They are ready-made type transformations for the most common patterns. Learn them once, use them forever.

---

## Partial<T> — make everything optional

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function updateUser(id: number, changes: Partial<User>): void {
  // changes can have any subset of User's properties
  console.log(`Updating ${id}:`, changes);
}

updateUser(1, { name: "Alice" });               // ✅ single field
updateUser(1, { email: "a@b.com", name: "A" }); // ✅ multiple fields
updateUser(1, {});                                // ✅ empty — all optional
```

**Use case:** Update/patch operations where only changed fields are sent.

---

## Required<T> — make everything required (remove optional)

```typescript
interface Config {
  apiUrl?: string;
  timeout?: number;
  retries?: number;
}

type StrictConfig = Required<Config>;
// { apiUrl: string; timeout: number; retries: number; }
```

---

## Readonly<T> — make everything readonly

```typescript
type ImmutableUser = Readonly<User>;

const user: ImmutableUser = { id: 1, name: "Alice", email: "a@b.com" };
user.name = "Bob"; // ❌ Cannot assign to 'name' because it is a read-only property
```

---

## Pick<T, K> — select specific properties

```typescript
type UserPreview = Pick<User, "id" | "name">;
// { id: number; name: string; }

function formatUserPreview(user: UserPreview): string {
  return `${user.id}: ${user.name}`;
}
```

**Use case:** Creating a subset type without defining a new interface.

---

## Omit<T, K> — exclude specific properties

```typescript
type UserWithoutEmail = Omit<User, "email">;
// { id: number; name: string; }

function createUser(data: Omit<User, "id">): User {
  return { id: Date.now(), ...data };
}
```

`Pick` and `Omit` are opposites. Use `Pick` when you want a small set of keys. Use `Omit` when you want most keys except a few.

---

## Record<K, T> — dictionary type

```typescript
type UserMap = Record<string, User>;

const users: UserMap = {
  "user_1": { id: 1, name: "Alice", email: "a@b.com" },
  "user_2": { id: 2, name: "Bob", email: "b@c.com" },
};

type StatusCode = 200 | 404 | 500;
type StatusMessages = Record<StatusCode, string>;

const messages: StatusMessages = {
  200: "OK",
  404: "Not Found",
  500: "Internal Server Error",
};
```

**Use case:** Objects used as maps/dictionaries with constrained keys.

---

## NonNullable<T> — remove null and undefined

```typescript
type Maybe = string | null | undefined;
type Definite = NonNullable<Maybe>; // string
```

---

## Extract<T, U> and Exclude<T, U> — filter unions

```typescript
type Mixed = string | number | boolean | null;

type StringsAndNumbers = Extract<Mixed, string | number>; // string | number
type Primitives = Exclude<Mixed, null>; // string | number | boolean
```

---

## Combining utilities

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  createdAt: Date;
}

// Common pattern: create payload type from entity type
type CreateProductPayload = Omit<Product, "id" | "createdAt">;
type UpdateProductPayload = Partial<Omit<Product, "id">>;
type ProductPreview = Pick<Product, "id" | "name" | "price">;
```

---

## Summary

| Utility | Effect |
|---------|--------|
| `Partial<T>` | All properties optional |
| `Required<T>` | All properties required |
| `Readonly<T>` | All properties readonly |
| `Pick<T, K>` | Keep only keys in K |
| `Omit<T, K>` | Remove keys in K |
| `Record<K, T>` | Object with keys K, values T |
| `NonNullable<T>` | Remove null/undefined |
| `Extract<T, U>` | Keep members in U |
| `Exclude<T, U>` | Remove members in U |

Your turn. Open DAY18/DRILL.md.
