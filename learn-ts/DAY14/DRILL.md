# DAY14: Drill — "Generic API Client"

Build a generic API client in `DAY14/`.

## File structure

```
DAY14/
  index.ts     — entry point with demo usage
  api.ts       — ApiClient class
  types.ts     — shared interfaces
```

## Requirements

### types.ts
```typescript
// Copy from DAY06 or create fresh:
export interface Todo {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
}

export interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

export interface ApiResponse<T> {
  data: T;
  status: number;
  ok: boolean;
}
```

### api.ts
- The `ApiClient` class from the README
- Add an error-handling wrapper: if `response.ok` is false, throw an error with the status

### index.ts
```typescript
// 1. Create an ApiClient pointing at https://jsonplaceholder.typicode.com
// 2. Fetch all todos (GET /todos) — print how many you got
// 3. Fetch a single todo with id 1 — print its title
// 4. Fetch posts for user 1 (GET /posts?userId=1) — print first 3 titles
// 5. Handle errors gracefully with try/catch
```

## Example output

```
Fetched 200 todos
Todo #1: "delectus aut autem" — completed: false
Posts by user 1:
  1. "sunt aut facere repellat provident occaecati excepturi optio reprehenderit"
  2. "qui est esse"
  3. "ea molestias quasi autem neque ut ipsum"
```

## Hints

<details>
<summary>Click for hints</summary>

- JSONPlaceholder is a free fake API — no auth needed
- `https://jsonplaceholder.typicode.com` is the base URL
- Query params are just added to the path like `?userId=1`
- `satisfies` keyword is optional; you can type the body argument directly
- Use `response.ok` to check for HTTP errors before parsing JSON
</details>

## Bonus

Add a `createTodo` method that POSTs a new todo and prints the response. Add a `patch` method for partial updates.

## How to run

```bash
cd ~/Dev/learn-ts/DAY14
npx tsx index.ts
```

## When you're done

Show me all three files.
