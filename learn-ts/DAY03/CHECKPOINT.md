# DAY03: Checkpoint

**Closed book.** No looking back.

1. What's the difference between an `interface` and a plain object literal?

2. In `interface Car { year?: number }`, what does the `?` mean?

3. How would you mark a property as "set once, never changed"?

4. What error does this produce?
   ```typescript
   interface Person { name: string; }
   const p: Person = { name: "Alice", age: 25 };
   ```

5. When would you use `type` instead of `interface`?

---

<details>
<summary>Answers</summary>

1. An `interface` is a reusable type definition. An object literal is a concrete value. The interface describes the shape; the object is the data.
2. The `year` property is optional — it can be omitted when creating the object.
3. Use `readonly` before the property name: `readonly id: string;`
4. `Object literal may only specify known properties, and 'age' does not exist in type 'Person'`
5. Use `type` for union types, primitive aliases, and tuples. Use `interface` for object shapes you might want to extend later.
</details>
