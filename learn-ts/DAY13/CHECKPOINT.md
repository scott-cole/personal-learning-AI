# DAY13: Checkpoint

**Closed book.** No looking back.

1. What does `document.querySelector<HTMLInputElement>("#email")` give you that plain `.querySelector` doesn't?

2. When would you use `as HTMLInputElement` instead of the generic `querySelector`?

3. What's the difference between `textContent` and `innerHTML`?

4. How do you prevent a form from reloading the page on submit?

5. What does the `!` (non-null assertion) do at the end of `document.getElementById("x")!`?

---

<details>
<summary>Answers</summary>

1. Proper TypeScript typing — `querySelector` returns `Element | null`, but the generic version returns `HTMLInputElement | null` with correct autocomplete for `.value`, `.checked`, etc.
2. When you already have an `Element` from some other method and need to assert a more specific type.
3. `textContent` sets plain text (safe, no HTML parsing). `innerHTML` parses HTML strings (XSS risk if user content is included).
4. Call `event.preventDefault()` in the submit event listener.
5. It tells TypeScript "this expression is definitely not null/undefined." If you're wrong, you get a runtime error instead of a compile-time check.
</details>
