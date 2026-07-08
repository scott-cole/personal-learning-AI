# DAY11: Async/Await and Promises

---

## "A Promise is a restaurant ticket"

You sit at a restaurant and order steak. The waiter brings you a ticket. This ticket is not the steak — it's a **promise** that the steak will come eventually.

While the kitchen works, you do other things: chat, check your phone, drink water. When the steak is ready, the waiter brings it. Your ticket has been **resolved** (fulfilled). If the kitchen burned down, the ticket would be **rejected**.

```typescript
const steakPromise: Promise<string> = new Promise((resolve, reject) => {
  // Kitchen is working...
  setTimeout(() => {
    resolve("Medium-rare steak, ready!");
    // or: reject("Kitchen is on fire!");
  }, 2000);
});

steakPromise
  .then((steak) => console.log(steak))
  .catch((error) => console.error(error));
```

---

## async/await — the pleasant syntax

`.then()` chains work, but `async/await` reads like synchronous code:

```typescript
async function eatDinner(): Promise<void> {
  const steak = await steakPromise;
  console.log(steak); // "Medium-rare steak, ready!"
}

eatDinner();
```

`await` pauses the function until the Promise resolves. The function must be marked `async`.

---

## Creating Promises

```typescript
function delay(ms: number): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => resolve(`Waited ${ms}ms`), ms);
  });
}

async function run(): Promise<void> {
  const result = await delay(1000);
  console.log(result); // "Waited 1000ms"
}

run();
```

The `resolve` function is called when the operation succeeds. `reject` is called on failure.

---

## Error handling with async/await

Wrap in try/catch:

```typescript
async function fetchUser(id: number): Promise<string> {
  if (id < 0) {
    throw new Error("Invalid ID");
  }
  return `User ${id}`;
}

async function main(): Promise<void> {
  try {
    const user = await fetchUser(-1);
    console.log(user);
  } catch (error) {
    console.error("Failed:", error.message);
  }
}

main();
```

---

## Parallel promises

When tasks are independent, run them in parallel with `Promise.all`:

```typescript
async function main(): Promise<void> {
  const [user, posts, comments] = await Promise.all([
    fetchUser(1),
    fetchPosts(1),
    fetchComments(1),
  ]);
  // All three run concurrently — much faster than sequential awaits
}
```

`Promise.all` rejects if **any** promise rejects. Use `Promise.allSettled` if you want all results regardless of failure.

---

## TypeScript and async

An `async` function always returns a `Promise`. The return type annotation should wrap the result:

```typescript
async function getNumber(): Promise<number> {
  return 42; // automatically wrapped in Promise.resolve(42)
}

const x = await getNumber(); // x is number, not Promise<number>
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Create a Promise | `new Promise<T>((res, rej) => ...)` |
| Await a promise | `const result = await somePromise` |
| Async function | `async function foo(): Promise<T>` |
| Error handling | `try { ... } catch (e) { ... }` |
| Parallel execution | `Promise.all([p1, p2])` |
| Wait for all settled | `Promise.allSettled([p1, p2])` |

Your turn. Open DAY11/DRILL.md.
