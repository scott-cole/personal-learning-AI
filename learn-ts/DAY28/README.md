# DAY28: React Context and Reducers with Types

---

## "Context is a town bulletin board"

A small town has a bulletin board in the town square. Anyone can post a notice: "Town meeting Thursday at 7pm." Anyone can read it. No one needs to knock on every door individually.

React Context is a bulletin board for your component tree. You post a value at the top (the provider), and any component below can read it without passing props through every level.

---

## Creating typed context

```typescript
import { createContext, useContext, ReactNode } from "react";

interface ThemeContextType {
  theme: "light" | "dark";
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);
```

The generic parameter to `createContext` defines the shape. Starting with `undefined` forces consumers to check for existence.

---

## Provider component

```typescript
interface ThemeProviderProps {
  children: ReactNode;
}

function ThemeProvider({ children }: ThemeProviderProps): JSX.Element {
  const [theme, setTheme] = useState<"light" | "dark">("light");

  const toggleTheme = () => {
    setTheme((t) => (t === "light" ? "dark" : "light"));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

---

## Custom hook for consuming

```typescript
function useTheme(): ThemeContextType {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error("useTheme must be used within a ThemeProvider");
  }
  return context;
}
```

This pattern ensures consumers get a useful error if they forget the provider.

---

## useReducer — dispatch with type safety

For complex state, `useReducer` is cleaner than multiple `useState` calls:

```typescript
interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

interface TodoState {
  todos: Todo[];
  loading: boolean;
}

type TodoAction =
  | { type: "ADD_TODO"; payload: { title: string } }
  | { type: "TOGGLE_TODO"; payload: { id: number } }
  | { type: "REMOVE_TODO"; payload: { id: number } }
  | { type: "SET_LOADING"; payload: boolean };

function todoReducer(state: TodoState, action: TodoAction): TodoState {
  switch (action.type) {
    case "ADD_TODO":
      return {
        ...state,
        todos: [
          ...state.todos,
          {
            id: Date.now(),
            title: action.payload.title,
            completed: false,
          },
        ],
      };
    case "TOGGLE_TODO":
      return {
        ...state,
        todos: state.todos.map((t) =>
          t.id === action.payload.id ? { ...t, completed: !t.completed } : t
        ),
      };
    case "REMOVE_TODO":
      return {
        ...state,
        todos: state.todos.filter((t) => t.id !== action.payload.id),
      };
    case "SET_LOADING":
      return { ...state, loading: action.payload };
  }
}
```

---

## Context + Reducer = global state

```typescript
interface TodoContextType {
  state: TodoState;
  dispatch: React.Dispatch<TodoAction>;
}

const TodoContext = createContext<TodoContextType | undefined>(undefined);

function TodoProvider({ children }: { children: ReactNode }): JSX.Element {
  const [state, dispatch] = useReducer(todoReducer, {
    todos: [],
    loading: false,
  });

  return (
    <TodoContext.Provider value={{ state, dispatch }}>
      {children}
    </TodoContext.Provider>
  );
}

function useTodo(): TodoContextType {
  const context = useContext(TodoContext);
  if (!context) throw new Error("useTodo must be used within TodoProvider");
  return context;
}
```

---

## Using the context

```typescript
function TodoList(): JSX.Element {
  const { state, dispatch } = useTodo();

  return (
    <div>
      {state.todos.map((todo) => (
        <div key={todo.id}>
          <input
            type="checkbox"
            checked={todo.completed}
            onChange={() =>
              dispatch({ type: "TOGGLE_TODO", payload: { id: todo.id } })
            }
          />
          <span>{todo.title}</span>
          <button
            onClick={() =>
              dispatch({ type: "REMOVE_TODO", payload: { id: todo.id } })
            }
          >
            ✕
          </button>
        </div>
      ))}
    </div>
  );
}
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Create context | `const Ctx = createContext<T>(undefined)` |
| Provider | `<Ctx.Provider value={val}>{children}</Ctx.Provider>` |
| Consumer hook | `function useCtx() { const c = useContext(Ctx); if (!c) throw ...; return c; }` |
| Action union | `type Action = \| { type: "ADD"; payload: T } \| ...` |
| useReducer | `useReducer(reducer, initialState)` |
| Dispatch type | `React.Dispatch<ActionType>` |

Your turn. Open DAY28/DRILL.md.
