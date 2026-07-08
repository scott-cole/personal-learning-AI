# DAY27: React Hooks with Types (useState, useEffect, Custom Hooks)

---

## "Hooks are utility belts"

Batman's utility belt has a grapple gun, Batarangs, smoke pellets, a flashlight — each tool clips onto the belt and handles one specific job. He reaches for the right tool when the situation calls for it.

React hooks are a utility belt for components. Each hook handles one specific concern: state (`useState`), side effects (`useEffect`), context (`useContext`), refs (`useRef`), and so on. Clip them onto your component as needed.

---

## useState — typed state

```typescript
const [count, setCount] = useState<number>(0);
const [name, setName] = useState<string>("");
const [items, setItems] = useState<string[]>([]);
const [user, setUser] = useState<User | null>(null);
```

TypeScript infers the type from the initial value, but you can be explicit with the generic parameter. Use unions for state that starts empty or `null`.

---

## useEffect — typed side effects

```typescript
useEffect(() => {
  // No return value — runs on mount and when count changes
  document.title = `Count: ${count}`;
}, [count]);

useEffect(() => {
  // With cleanup — runs on unmount
  const timer = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => clearInterval(timer); // cleanup function
}, []);
```

The callback's return type is `void | (() => void)`. TypeScript checks that you return either nothing or a cleanup function.

---

## useRef — typed refs

```typescript
function TextInputWithFocus(): JSX.Element {
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    inputRef.current?.focus(); // optional chaining — might be null
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

`useRef<HTMLInputElement>(null)` creates a ref with type `React.RefObject<HTMLInputElement | null>`. The `.current` property is typed accordingly.

For mutable values (not DOM refs):

```typescript
const intervalRef = useRef<number | null>(null);
intervalRef.current = setInterval(() => {}, 1000);
```

---

## Custom hooks — extracting reusable logic

Custom hooks are just functions that use other hooks. They follow the `use*` naming convention.

```typescript
// useCounter.ts
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  const increment = () => setCount((c) => c + 1);
  const decrement = () => setCount((c) => c - 1);
  const reset = () => setCount(initialValue);

  return { count, increment, decrement, reset };
}

// Usage:
function Counter(): JSX.Element {
  const { count, increment, decrement, reset } = useCounter(10);
  // ...
}
```

---

## Custom hook with types

```typescript
interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    let cancelled = false;
    fetch(url)
      .then((res) => res.json())
      .then((result) => {
        if (!cancelled) {
          setData(result);
          setLoading(false);
        }
      })
      .catch((err) => {
        if (!cancelled) {
          setError(err.message);
          setLoading(false);
        }
      });
    return () => { cancelled = true; };
  }, [url]);

  return { data, loading, error };
}

// Usage:
interface User { name: string; email: string; }
const { data, loading } = useFetch<User>("/api/users/1");
```

---

## Common custom hooks

```typescript
function useLocalStorage<T>(key: string, initial: T): [T, (val: T) => void] {
  const [stored, setStored] = useState<T>(() => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initial;
    } catch {
      return initial;
    }
  });

  const setValue = (value: T) => {
    setStored(value);
    localStorage.setItem(key, JSON.stringify(value));
  };

  return [stored, setValue];
}
```

---

## Summary

| Hook | Type Pattern |
|------|-------------|
| `useState<T>` | `useState<T>(initial)` |
| `useEffect` | `useEffect(fn, deps)` — return cleanup |
| `useRef` DOM | `useRef<HTMLInputElement>(null)` |
| `useRef` value | `useRef<number>(0)` |
| Custom hook | Named `useXxx`, returns typed object/array |
| Generic hook | `function useFetch<T>(url: string)` |

Your turn. Open DAY27/DRILL.md.
