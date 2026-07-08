# DAY25: Testing with Vitest

---

## "Tests are a safety net"

A trapeze artist performs flips high above the crowd. Below is a safety net. If they miss the catch, the net catches them. They perform *better* because they know the net is there.

Tests are a safety net for your code. When you refactor, add features, or fix bugs, the tests catch you if something breaks. You code *better* because you know the net is there.

---

## Setting up Vitest

```bash
npm install -D vitest typescript @types/node
```

Add to `package.json`:

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest"
  }
}
```

Vitest discovers files matching `*.test.ts` or `*.spec.ts` automatically.

---

## Your first test

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

export function divide(a: number, b: number): number {
  if (b === 0) throw new Error("Division by zero");
  return a / b;
}
```

```typescript
// math.test.ts
import { describe, it, expect } from "vitest";
import { add, divide } from "./math";

describe("add", () => {
  it("adds two positive numbers", () => {
    expect(add(2, 3)).toBe(5);
  });

  it("adds negative numbers", () => {
    expect(add(-1, -1)).toBe(-2);
  });
});

describe("divide", () => {
  it("divides two numbers", () => {
    expect(divide(10, 2)).toBe(5);
  });

  it("throws on division by zero", () => {
    expect(() => divide(10, 0)).toThrow("Division by zero");
  });
});
```

---

## Matchers

```typescript
expect(value).toBe(42);            // strict equality (===)
expect(value).toEqual({ a: 1 });   // deep equality
expect(value).toBeNull();
expect(value).toBeDefined();
expect(value).toBeTruthy();
expect(value).toBeFalsy();

expect(array).toContain("item");
expect(string).toMatch(/regex/);

expect(fn).toThrow();
expect(fn).toThrow("message");

expect(response).toHaveProperty("data.users", expect.any(Array));
```

---

## Testing async code

```typescript
import { describe, it, expect } from "vitest";

async function fetchUser(id: number): Promise<{ name: string }> {
  return { name: "Alice" };
}

describe("fetchUser", () => {
  it("returns a user", async () => {
    const user = await fetchUser(1);
    expect(user.name).toBe("Alice");
  });
});
```

---

## Testing with mocks

```typescript
import { vi, describe, it, expect } from "vitest";

const mockApi = {
  get: vi.fn().mockResolvedValue({ data: { id: 1, name: "Alice" } }),
};

describe("ApiClient", () => {
  it("calls the API with the correct URL", async () => {
    const result = await mockApi.get("/users/1");
    expect(mockApi.get).toHaveBeenCalledWith("/users/1");
    expect(result.data.name).toBe("Alice");
  });
});
```

---

## File naming conventions

Vitest picks up:
- `*.test.ts`
- `*.spec.ts`
- `tests/*.ts`

Put tests next to the source file or in a `tests/` directory — either works.

---

## Running tests

```bash
npm test            # run once
npm run test:watch  # watch mode — re-runs on file changes
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Test block | `describe("name", () => { it("does X", () => { ... }) })` |
| Assertion | `expect(actual).toBe(expected)` |
| Deep equality | `expect(obj).toEqual(expected)` |
| Error check | `expect(() => fn()).toThrow()` |
| Async test | `it("works", async () => { await ... })` |
| Mock function | `vi.fn()` |
| Run tests | `vitest run` / `vitest` (watch) |

Your turn. Open DAY25/DRILL.md.
