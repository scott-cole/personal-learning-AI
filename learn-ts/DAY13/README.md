# DAY13: DOM Manipulation with TypeScript

---

## "The DOM is a treehouse"

Imagine a treehouse built in a giant oak. The trunk is the root (`document`). Branches split off into limbs (`<html>`, `<body>`). Smaller branches hold leaves (`<div>`, `<p>`, `<button>`). Each leaf has its own color, shape, and position.

The DOM (Document Object Model) is exactly that — a tree of elements. TypeScript helps you navigate and modify this tree without falling off a branch.

---

## Setting up a browser environment

Create an HTML file and a TypeScript file. Use a bundler like Vite, or compile with tsc and include the JS file in a script tag.

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head><title>TS DOM</title></head>
<body>
  <div id="app"></div>
  <script src="index.js"></script>
</body>
</html>
```

```typescript
// index.ts
const app = document.getElementById("app");
if (app) {
  app.innerHTML = "<h1>Hello from TypeScript!</h1>";
}
```

---

## Selecting elements

```typescript
const button = document.querySelector<HTMLButtonElement>("#submitBtn");
const inputs = document.querySelectorAll<HTMLInputElement>(".field");
const element = document.getElementById("main");

// querySelector with generic type
const heading = document.querySelector<HTMLHeadingElement>("h1");
```

The generic type parameter tells TypeScript what kind of element you expect. This gives you proper autocomplete: `button?.click()`, `input?.value`, etc.

---

## Creating and appending elements

```typescript
const list = document.getElementById("todo-list");
if (!list) throw new Error("List not found");

const item = document.createElement("li");
item.textContent = "Buy groceries";
item.className = "todo-item";
item.addEventListener("click", () => {
  item.classList.toggle("completed");
});

list.appendChild(item);
```

---

## Event handling with types

```typescript
const form = document.querySelector<HTMLFormElement>("#login-form");
form?.addEventListener("submit", (event: SubmitEvent) => {
  event.preventDefault();

  const emailInput = document.querySelector<HTMLInputElement>("#email");
  const passwordInput = document.querySelector<HTMLInputElement>("#password");

  if (emailInput && passwordInput) {
    console.log("Email:", emailInput.value);
    console.log("Password:", passwordInput.value);
  }
});
```

The `SubmitEvent` type gives you `event.preventDefault()` and other event-specific properties.

---

## Type assertion (when you know better)

Sometimes TypeScript doesn't know the specific type. You can tell it:

```typescript
const input = document.getElementById("email") as HTMLInputElement;
input.value = "test@example.com";

// Or the angle-bracket syntax (less common in TSX files)
const canvas = <HTMLCanvasElement>document.getElementById("myCanvas");
```

Use assertions sparingly — they bypass type checking. Prefer `querySelector<HTMLInputElement>` when possible.

---

## A complete example

```typescript
interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

class TodoApp {
  private list: HTMLUListElement;
  private input: HTMLInputElement;

  constructor() {
    this.list = document.querySelector<HTMLUListElement>("#todo-list")!;
    this.input = document.querySelector<HTMLInputElement>("#todo-input")!;
    this.input.addEventListener("keypress", (e) => {
      if (e.key === "Enter") this.add();
    });
  }

  add(): void {
    const todo: Todo = {
      id: Date.now(),
      title: this.input.value,
      completed: false,
    };
    const li = document.createElement("li");
    li.textContent = todo.title;
    this.list.appendChild(li);
    this.input.value = "";
  }
}

const app = new TodoApp();
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Select element | `document.querySelector<T>("selector")` |
| Get by id | `document.getElementById("id")` |
| Create element | `document.createElement("div")` |
| Add event | `element.addEventListener("click", fn)` |
| Type assertion | `el as HTMLInputElement` |
| Text content | `element.textContent = "hello"` |
| Class toggle | `element.classList.toggle("active")` |

Your turn. Open DAY13/DRILL.md.
