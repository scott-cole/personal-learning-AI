# DAY26: Checkpoint

**Closed book.** No looking back.

1. What is the purpose of the interface before a component's function declaration?

2. How do you make a prop optional?

3. What type would you use for a `<button>` click handler parameter?

4. What does `React.ReactNode` represent?

5. Why is `useState<User | null>(null)` preferred over `useState({} as User)`?

---

<details>
<summary>Answers</summary>

1. It defines the shape of the props object, giving TypeScript the information it needs to check prop usage at compile time.
2. Add `?` after the property name: `age?: number`.
3. `React.MouseEvent<HTMLButtonElement>`.
4. Any renderable React content — strings, numbers, JSX elements, arrays, fragments, or `null`/`undefined`.
5. Because `{} as User` is a type assertion that lies to TypeScript — the object isn't actually a User. Using `User | null` accurately reflects that the state starts as "no user selected."
</details>
