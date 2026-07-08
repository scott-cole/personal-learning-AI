# DAY12: Error Handling (try/catch, throw, Result pattern)

---

## "Error handling is car insurance"

You drive your car every day. Most days, nothing bad happens. But you still buy insurance — because when something *does* go wrong, you want a plan, not a panic.

Error handling in code is the same. Most of the time, functions succeed. But when they don't, you need a structured way to handle it — not a crash.

---

## throw — causing a controlled failure

When something goes wrong, `throw` signals the problem:

```typescript
function divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error("Division by zero is not allowed");
  }
  return a / b;
}
```

`new Error("message")` creates an error object with a `.message` property and a stack trace.

---

## try/catch — catching the thrown ball

```typescript
try {
  const result = divide(10, 0);
  console.log(result); // never reached
} catch (error) {
  if (error instanceof Error) {
    console.error("Oops:", error.message); // "Oops: Division by zero is not allowed"
  }
}
```

The `catch` block runs only if something in `try` throws. The error variable is typed as `unknown` in modern TypeScript — you must narrow it before using properties.

---

## finally — cleanup that always runs

```typescript
function readFile(path: string): string {
  try {
    // open file, read contents...
    return "file contents";
  } catch (error) {
    console.error("Failed to read file");
    return "";
  } finally {
    console.log("Closing file handle..."); // always runs, even after return
  }
}
```

`finally` runs regardless of success or failure — perfect for closing connections, freeing memory, etc.

---

## The Result pattern — a functional alternative

Throwing exceptions is uncontrolled — any caller might forget try/catch. The Result pattern makes errors a return value:

```typescript
type Result<T> = { ok: true; value: T } | { ok: false; error: string };

function safeDivide(a: number, b: number): Result<number> {
  if (b === 0) {
    return { ok: false, error: "Division by zero" };
  }
  return { ok: true, value: a / b };
}

function main(): void {
  const result = safeDivide(10, 2);

  if (result.ok) {
    console.log("Result:", result.value); // narrowed to { ok: true, value: number }
  } else {
    console.error("Error:", result.error); // narrowed to { ok: false, error: string }
  }
}
```

This forces the caller to handle both paths — no uncaught exceptions.

---

## Custom error classes

```typescript
class ValidationError extends Error {
  constructor(message: string, public field: string) {
    super(message);
    this.name = "ValidationError";
  }
}

function validateEmail(email: string): void {
  if (!email.includes("@")) {
    throw new ValidationError("Invalid email format", "email");
  }
}

try {
  validateEmail("not-an-email");
} catch (error) {
  if (error instanceof ValidationError) {
    console.error(`${error.field}: ${error.message}`); // "email: Invalid email format"
  }
}
```

---

## Best practices

- **Only catch errors you can handle.** Let the rest bubble up.
- **Use specific error types** (`ValidationError`, `NetworkError`) rather than generic `Error`.
- **Always narrow `error` in `catch`** — it's `unknown` for a reason.
- **Consider the Result pattern** for operations where failure is a normal outcome (validation, parsing).

---

## Summary

| Concept | Syntax |
|---------|--------|
| Throw | `throw new Error("msg")` |
| Try/catch | `try { ... } catch (e) { ... }` |
| Finally | `finally { cleanup() }` |
| Result type | `type Result<T> = { ok: true; value: T } \| { ok: false; error: string }` |
| Custom error | `class MyError extends Error {}` |
| Narrow in catch | `if (e instanceof Error)` |

Your turn. Open DAY12/DRILL.md.
