# DAY16: Checkpoint

**Closed book.** No looking back.

1. Write a mapped type `Optional<T>` that makes all properties optional (don't use the built-in).

2. What does `-readonly` do in a mapped type?

3. What does the `as` clause do in `[K in keyof T as NewKey]`?

4. How do you iterate over only the keys whose values are strings?

5. Given `{ [K in keyof T]: string }`, what happens to a property `age: number`?

---

<details>
<summary>Answers</summary>

1. `type Optional<T> = { [K in keyof T]?: T[K]; }`
2. It removes the `readonly` modifier, making the property writable.
3. It remaps the key — you can rename, filter (via `never`), or transform key names.
4. `{ [K in keyof T as T[K] extends string ? K : never]: T[K]; }`
5. `age` still exists, but its type becomes `string` instead of `number` (the value type is replaced).
</details>
