# DAY28: Checkpoint

**Closed book.** No looking back.

1. What problem does React Context solve?

2. Why does the custom hook `useTheme` throw an error if context is `undefined`?

3. What is the advantage of `useReducer` over multiple `useState` calls?

4. How do you type a dispatch function from `useReducer`?

5. What makes `TodoAction` a discriminated union?

---

<details>
<summary>Answers</summary>

1. Prop drilling — passing props through many levels of components that don't need them. Context lets a value be available to any descendant without explicit prop passing.
2. It provides a clear error message if a component tries to use the context outside of its provider, instead of silently returning `undefined` and causing cryptic errors later.
3. It centralizes state logic in a single reducer function, makes action types explicit and traceable, and is easier to test and debug (especially with Redux DevTools).
4. `React.Dispatch<ActionType>` where `ActionType` is the union of all possible actions.
5. The `type` property (a string literal `"ADD"`, `"TOGGLE"`, `"REMOVE"`) acts as a discriminant — TypeScript narrows the action based on the `type` value, giving type-safe access to `payload`.
</details>
