# DAY14: Checkpoint

**Closed book.** No looking back.

1. Why is `request<T>` generic instead of returning `any`?

2. What does `ApiResponse<T>` contain beyond just the data?

3. How would you add query parameters like `?userId=1` to the client?

4. What happens if `fetch` gets a network error?

5. Why is `satisfies CreateUserPayload` useful when passing an object literal?

---

<details>
<summary>Answers</summary>

1. So each call site can specify the expected response type (`get<User[]>`), and TypeScript checks that you use the data correctly.
2. The response `status` number and an `ok` boolean flag indicating HTTP success.
3. Concatenate them to the path string: `get<T>(\`/posts?userId=${userId}\`)`. A more advanced client would accept a params object.
4. `fetch` rejects the promise with a `TypeError`. The `catch` block in the calling code handles it.
5. It preserves the full literal type for excess property checking while still confirming it matches the interface — better than a type annotation which strips excess properties.
</details>
