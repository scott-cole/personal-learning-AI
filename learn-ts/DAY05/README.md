# DAY05: Union Types, Type Aliases, and Literal Types

---

## "Union types are a multi-tool"

A multi-tool has pliers, a knife, a screwdriver, and scissors all in one device. You carry one tool that can be many things depending on what you need.

A union type is the same idea: a value that can be one of several types.

```typescript
let id: string | number;
id = "abc-123"; // ✅ string is allowed
id = 42;        // ✅ number is allowed
id = true;      // ❌ Type 'boolean' is not assignable to type 'string | number'
```

The `|` means "or". `string | number` reads as "string or number."

---

## Why unions matter

APIs return different shapes depending on success or failure. User input might be a string or already parsed. Union types model this elegantly:

```typescript
function formatId(id: string | number): string {
  return `ID: ${id}`;
}

console.log(formatId(123));    // "ID: 123"
console.log(formatId("abc"));  // "ID: abc"
```

Inside the function, TypeScript lets you use `id` in ways that work for both types (like template literals). To use string-specific methods or number-specific methods, you need **type narrowing** (DAY09).

---

## Type aliases — giving unions a name

You don't want to write `string | number` everywhere. Give it a name:

```typescript
type ID = string | number;

let userId: ID = "user_001";
let postId: ID = 99;

function lookup(id: ID): void {
  console.log(`Looking up ${id}`);
}
```

Type aliases are like nicknames for types. They make your code read like a story: `ID`, `JSONValue`, `StatusCode`.

```typescript
type Status = "pending" | "active" | "completed";
type Coordinates = { lat: number; lng: number };
type Callback = (err: Error | null, data: string) => void;
```

---

## Literal types — pinning down exact values

Sometimes you don't just want "a string" — you want one of three specific strings:

```typescript
type Direction = "north" | "south" | "east" | "west";

function move(direction: Direction): void {
  console.log(`Moving ${direction}`);
}

move("north"); // ✅
move("up");    // ❌ Argument of type '"up"' is not assignable to parameter of type 'Direction'
```

The values `"north"`, `"south"`, etc. are **literal types** — they are the only allowed values. TypeScript treats them as specific types, not generic strings.

```typescript
type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;
type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE";
type YesNo = "yes" | "no";
```

---

## Combining unions with interfaces

This is where TypeScript really shines — modeling real-world states:

```typescript
interface Success {
  status: "success";
  data: string;
}

interface Error {
  status: "error";
  message: string;
}

type ApiResponse = Success | Error;

function handleResponse(res: ApiResponse): void {
  if (res.status === "success") {
    console.log("Data:", res.data);
  } else {
    console.log("Error:", res.message);
  }
}
```

This is called a **discriminated union** — the `status` field tells you which shape the rest of the object has. We'll use this pattern constantly.

---

## Type aliases for primitives

You can even alias primitives for clarity:

```typescript
type Email = string;
type PhoneNumber = string;
type Age = number;

let userEmail: Email = "test@example.com";
```

This doesn't add validation — `Email` is still just a `string` — but it documents intent and makes refactoring easier.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Union type | `string \| number` |
| Type alias | `type ID = string \| number` |
| Literal type | `type Direction = "north" \| "south"` |
| Discriminated union | `type Resp = { kind: "ok"; data: T } \| { kind: "err"; msg: string }` |
| Primitive alias | `type Email = string` |

Your turn. Open DAY05/DRILL.md.
