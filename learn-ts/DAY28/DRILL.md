# DAY28: Drill — "Theme + Todo with Context"

Build an app with Context + Reducer in `DAY28/`. Use Vite.

## Setup

```bash
cd ~/Dev/learn-ts/DAY28
npm create vite@latest . -- --template react-ts
npm install
```

## File structure

```
src/
  context/
    ThemeContext.tsx
    TodoContext.tsx
  components/
    ThemeToggle.tsx
    TodoInput.tsx
    TodoList.tsx
    Header.tsx
  App.tsx
  App.css
  types.ts
```

## Requirements

### types.ts
```typescript
export interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

export type TodoAction =
  | { type: "ADD"; payload: { title: string } }
  | { type: "TOGGLE"; payload: { id: number } }
  | { type: "REMOVE"; payload: { id: number } };
```

### ThemeContext.tsx
- Stores `theme: "light" | "dark"` and `toggleTheme()`
- Applies a `data-theme` attribute on the root `<html>` element

### TodoContext.tsx
- `useReducer` with `todos: Todo[]` initial empty
- Exposes `todos` and `dispatch`
- Action types: ADD, TOGGLE, REMOVE

### Components
- **ThemeToggle**: button that toggles theme (☀️/🌙)
- **TodoInput**: text input + "Add" button — dispatches ADD
- **TodoList**: renders todos, checkbox toggles, remove button
- **Header**: shows app title + ThemeToggle

### App.tsx
- Wraps everything in ThemeProvider and TodoProvider
- Renders Header, TodoInput, TodoList

## Example interaction

```
Type "Buy milk" in input → click Add
→ "Buy milk" appears in the list with ☐ checkbox

Click ☐ → line-through, checked
Click ⊖ → item disappears

Click theme toggle → dark mode (bg darkens, text lightens)
```

## Hints

<details>
<summary>Click for hints</summary>

- `TodoAction` is a discriminated union — the `type` property narrows the action
- Use `useContext` in `useTodo` and `useTheme` custom hooks
- For dark mode, set `document.documentElement.setAttribute("data-theme", theme)`
- CSS uses `[data-theme="dark"]` selectors to toggle styles
- `React.Dispatch<TodoAction>` is the type for the dispatch function
</details>

## Bonus

Add a "Clear completed" button that dispatches a new `CLEAR_COMPLETED` action. Add localStorage persistence for both theme and todos.

## When you're done

Show me your files and a screenshot of the app with both themes visible.
