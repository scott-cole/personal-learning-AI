# DAY14: API Client with Generics (Project)

---

## "An API client is a universal remote"

You have a TV, a soundbar, a streaming box, and a game console. Four remotes on the coffee table. Then you buy a universal remote — one device that controls everything. Point, press, done.

A generic API client is a universal remote for HTTP endpoints. One client, any backend, strongly typed.

---

## What we're building

A reusable HTTP client that can:

```
GET    /users        → User[]
GET    /users/1      → User
POST   /users        → User
PUT    /users/1      → User
DELETE /users/1      → void
```

With full type safety — no `any`, no manual parsing.

---

## The core client

```typescript
// api.ts
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";

interface ApiConfig {
  baseUrl: string;
  headers?: Record<string, string>;
}

interface ApiResponse<T> {
  data: T;
  status: number;
  ok: boolean;
}

class ApiClient {
  constructor(private config: ApiConfig) {}

  private async request<T>(
    method: HttpMethod,
    path: string,
    body?: unknown
  ): Promise<ApiResponse<T>> {
    const url = `${this.config.baseUrl}${path}`;
    const options: RequestInit = {
      method,
      headers: {
        "Content-Type": "application/json",
        ...this.config.headers,
      },
    };

    if (body) {
      options.body = JSON.stringify(body);
    }

    const response = await fetch(url, options);
    const data = await response.json() as T;

    return {
      data,
      status: response.status,
      ok: response.ok,
    };
  }

  get<T>(path: string): Promise<ApiResponse<T>> {
    return this.request<T>("GET", path);
  }

  post<T>(path: string, body: unknown): Promise<ApiResponse<T>> {
    return this.request<T>("POST", path, body);
  }

  put<T>(path: string, body: unknown): Promise<ApiResponse<T>> {
    return this.request<T>("PUT", path, body);
  }

  delete(path: string): Promise<ApiResponse<void>> {
    return this.request<void>("DELETE", path);
  }
}
```

---

## Using the client

```typescript
// types.ts
interface User {
  id: number;
  name: string;
  email: string;
}

interface CreateUserPayload {
  name: string;
  email: string;
}

// main.ts
const api = new ApiClient({ baseUrl: "https://jsonplaceholder.typicode.com" });

async function main(): Promise<void> {
  const users = await api.get<User[]>("/users");
  console.log(users.data); // typed as User[]

  const user = await api.get<User>("/users/1");
  console.log(user.data.name); // typed as User

  const newUser = await api.post<User>("/users", {
    name: "Scott",
    email: "scott@example.com",
  } satisfies CreateUserPayload);
}

main();
```

---

## Week 2 recap

| Day | Skill | Used in project |
|-----|-------|----------------|
| DAY08 | Generics | `request<T>`, `ApiResponse<T>` |
| DAY09 | Enums, narrowing | `HttpMethod` enum, response handling |
| DAY10 | Modules | `api.ts`, `types.ts`, `main.ts` |
| DAY11 | Async/await | Every method returns `Promise` |
| DAY12 | Error handling | Try/catch in request, error responses |
| DAY13 | DOM (optional) | Could build a UI for this |

Your turn. Open DAY14/DRILL.md.
