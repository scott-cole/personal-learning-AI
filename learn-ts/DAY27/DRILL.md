# DAY27: Drill — "Custom Hooks"

Build React components with custom hooks in `DAY27/`. Use Vite.

## Setup

```bash
cd ~/Dev/learn-ts/DAY27
npm create vite@latest . -- --template react-ts
npm install
```

## File structure

```
src/
  hooks/
    useCounter.ts
    useLocalStorage.ts
    useFetch.ts
  components/
    Counter.tsx
    Settings.tsx
    UserProfile.tsx
  App.tsx
  App.css
  types.ts
```

## Requirements

### types.ts
```typescript
export interface User {
  id: number;
  name: string;
  email: string;
}
```

### hooks/useCounter.ts
```typescript
// Returns { count, increment, decrement, reset }
// Accept optional initial value (default 0) and step (default 1)
```

### hooks/useLocalStorage.ts
```typescript
// Generic hook: useLocalStorage<T>(key: string, initial: T)
// Returns [value, setValue] — persisted to localStorage
```

### hooks/useFetch.ts
```typescript
// Generic hook: useFetch<T>(url: string)
// Returns { data: T | null, loading: boolean, error: string | null }
```

### components/Counter.tsx
- Uses `useCounter` with step control
- Shows count, +/step buttons, reset

### components/Settings.tsx
- Uses `useLocalStorage` for theme ("light" | "dark") and username
- Dropdown for theme, input for username
- Shows current settings

### components/UserProfile.tsx
- Uses `useFetch<User>` with URL `https://jsonplaceholder.typicode.com/users/1`
- Shows loading spinner, user data, or error message

### App.tsx
- Renders all three components
- At least one CSS style to show theming works

## Hints

<details>
<summary>Click for hints</summary>

- `useCounter` returns an object, not an array (clearer for 4+ values)
- `useLocalStorage` returns a tuple like `useState`
- `useFetch` should use `useEffect` with the URL as dependency
- For the theme, apply a class to the root div: `className={theme}`
- Cleanup the fetch with an `AbortController` or a `cancelled` flag
</details>

## Bonus

Add a `useDebounce<T>(value: T, delay: number): T` hook that debounces a value. Use it in the Settings component to debounce the username input before saving to localStorage.

## When you're done

Show me your hook implementations and a screenshot of the running app.
