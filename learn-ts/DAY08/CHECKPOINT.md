# DAY08: Checkpoint

**Closed book.** No looking back.

1. What does `<T>` mean in `function identity<T>(value: T): T`?

2. Why is `any` worse than a generic parameter?

3. Write a generic constraint that requires `T` to have a `.length` property.

4. What's the difference between `T[]` and `Array<T>`?

5. Given `class Stack<T>`, what type is `new Stack<number>().pop()`?

---

<details>
<summary>Answers</summary>

1. A type parameter — a placeholder for a concrete type that's filled in when the function is called.
2. `any` disables type checking entirely. Generics preserve the specific type through the function.
3. `function foo<T extends { length: number }>(x: T)`
4. Nothing — they are equivalent syntax. `T[]` is more common; `Array<T>` is useful in complex expressions.
5. `number | undefined` (because the stack might be empty).
</details>
