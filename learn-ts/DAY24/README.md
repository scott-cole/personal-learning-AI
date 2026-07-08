# DAY24: Zod Validation

---

## "Zod is a bouncer with a list"

A nightclub has a bouncer at the door. Before anyone enters, the bouncer checks: "Are you on the list? Is your ID valid? Are you old enough?" Only people who pass all checks get in.

Zod is a bouncer for your data. You define a **schema** (the list), and Zod checks if incoming data matches. If it does, you get clean typed data. If not, you get detailed errors.

---

## Why not just TypeScript?

TypeScript only exists at compile time. At runtime, `any` can slip in from an API, a file, or user input. Zod runs at **runtime**:

```typescript
import { z } from "zod";

const UserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  age: z.number().int().positive().optional(),
});

// Inferred type from the schema
type User = z.infer<typeof UserSchema>;
// { name: string; email: string; age?: number | undefined }
```

---

## Basic validation

```typescript
const result = UserSchema.safeParse({ name: "A", email: "not-an-email" });

if (!result.success) {
  console.log(result.error.format());
  // {
  //   name: { _errors: ["String must contain at least 2 character(s)"] },
  //   email: { _errors: ["Invalid email"] },
  // }
}
```

`safeParse` returns a discriminated union: `{ success: true; data: T } | { success: false; error: ZodError }`.

Use `.parse()` if you want it to throw on failure:

```typescript
const user = UserSchema.parse(input); // throws if invalid
```

---

## Common Zod types

```typescript
z.string()
z.string().email()
z.string().url()
z.string().min(3).max(100)
z.string().regex(/^[a-z]+$/)

z.number()
z.number().int()
z.number().positive()
z.number().min(0).max(100)

z.boolean()

z.array(z.string())        // string[]
z.array(z.number()).min(1) // non-empty number[]

z.enum(["pending", "active", "disabled"])

z.union([z.string(), z.number()]) // string | number

z.literal("admin")
z.nullable(z.string())     // string | null
z.optional(z.string())     // string | undefined
```

---

## Object validation

```typescript
const ProductSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(1),
  price: z.number().positive(),
  tags: z.array(z.string()).optional(),
  inStock: z.boolean().default(true),
});

type Product = z.infer<typeof ProductSchema>;
```

`.default()` fills in a value if the field is missing.

---

## Nested objects

```typescript
const OrderSchema = z.object({
  id: z.string().uuid(),
  items: z.array(
    z.object({
      productId: z.string().uuid(),
      quantity: z.number().int().positive(),
    })
  ).min(1),
  total: z.number().positive(),
});
```

---

## Using Zod with Express

```typescript
import { z } from "zod";
import { Request, Response } from "express";

const CreateUserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  age: z.number().int().positive().optional(),
});

app.post("/users", (req: Request, res: Response) => {
  const result = CreateUserSchema.safeParse(req.body);

  if (!result.success) {
    return res.status(400).json({
      error: "Validation failed",
      details: result.error.flatten().fieldErrors,
    });
  }

  // result.data is fully typed
  const { name, email, age } = result.data;
  // ...
});
```

---

## Custom error messages

```typescript
const schema = z.object({
  email: z.string().email("Please provide a valid email address"),
  age: z.number().int("Age must be a whole number").positive("Age must be positive"),
});
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Define schema | `const S = z.object({ ... })` |
| Infer type | `type T = z.infer<typeof S>` |
| Safe parse | `S.safeParse(data)` |
| Unsafe parse | `S.parse(data)` (throws) |
| String validation | `z.string().min(2).max(100)` |
| Number validation | `z.number().int().positive()` |
| Enums | `z.enum(["a", "b"])` |
| Default value | `z.string().default("hello")` |

Your turn. Open DAY24/DRILL.md.
