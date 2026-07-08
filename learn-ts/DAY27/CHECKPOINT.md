# DAY27: Checkpoint

**Closed book.** No looking back.

1. Why do custom hooks need to start with `use`?

2. What's the return type of `useRef<HTMLInputElement>(null)`?

3. How do you type a generic custom hook like `useFetch<T>`?

4. What type should `useEffect`'s cleanup function return?

5. How would you type `useState` when the initial value is `null` but will later hold an object?

---

<details>
<summary>Answers</summary>

1. It's a convention that tells React (and the lint rules) that the function uses hooks inside it. The React lint rules check for this.
2. `React.RefObject<HTMLInputElement | null>`. The `.current` is typed as `HTMLInputElement | null`.
3. `function useFetch<T>(url: string): { data: T | null; loading: boolean; error: string | null }`
4. A function: `() => void`. You return a cleanup function that React calls on unmount.
5. `useState<User | null>(null)` — the type parameter is the union, so you can set it to either `null` or a `User` object.
</details>
