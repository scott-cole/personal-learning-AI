# DAY24: Drill — "Validation Middleware"

Build a validation layer with Zod in `DAY24/`.

## Setup

```bash
cd ~/Dev/learn-ts/DAY24
npm init -y
npm install express zod
npm install -D typescript @types/node @types/express tsx
```

## File structure

```
DAY24/
  package.json
  tsconfig.json
  src/
    index.ts          — Express app with Zod validation
    schemas.ts        — Zod schemas
    types.ts          — (optional, inferred from schemas)
```

## Requirements

### schemas.ts
```typescript
import { z } from "zod";

export const CreateUserSchema = z.object({
  name: z.string().min(2, "Name must be at least 2 characters"),
  email: z.string().email("Invalid email format"),
  role: z.enum(["admin", "user", "viewer"]).default("user"),
});

export const UpdateUserSchema = z.object({
  name: z.string().min(2).optional(),
  email: z.string().email().optional(),
  role: z.enum(["admin", "user", "viewer"]).optional(),
});

export const CreateProductSchema = z.object({
  name: z.string().min(1),
  price: z.number().positive("Price must be positive"),
  category: z.string().min(1),
  tags: z.array(z.string()).optional(),
});

export type CreateUser = z.infer<typeof CreateUserSchema>;
export type UpdateUser = z.infer<typeof UpdateUserSchema>;
export type CreateProduct = z.infer<typeof CreateProductSchema>;
```

### index.ts
```typescript
// POST /users — validate with CreateUserSchema
// PUT /users/:id — validate with UpdateUserSchema
// POST /products — validate with CreateProductSchema
// Each endpoint returns proper 400 errors with field-level details
// Use result.error.flatten().fieldErrors for readable output
```

## Example output

```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"A", "email":"bad"}'

# Response:
# {
#   "error": "Validation failed",
#   "details": {
#     "name": ["Name must be at least 2 characters"],
#     "email": ["Invalid email format"]
#   }
# }

curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}'

# Response:
# { "id": 1, "name": "Alice", "email": "alice@example.com", "role": "user" }
```

## Hints

<details>
<summary>Click for hints</summary>

- Create a helper function `validate<T>(schema: z.ZodSchema<T>, data: unknown)` that returns typed data or throws
- Or use `safeParse` in the route handler
- `result.error.flatten()` returns `{ fieldErrors: {}, formErrors: [] }`
- Store created users/products in memory arrays
- Generate IDs with `crypto.randomUUID()` or `Date.now()`
</details>

## Bonus

Create a reusable `validate` middleware function: `const validate = (schema: ZodSchema) => (req, res, next) => { ... }`.

## When you're done

Show me all your files and a curl session testing validation errors and success.
