# DAY26: React with TypeScript (Props, State)

---

## "React components are Lego bricks"

A Lego castle is built from hundreds of bricks. A 2x4 brick, a sloped roof brick, a window brick, a door brick. Each brick has a specific shape and connects to others through standardized studs.

React components are Lego bricks. Each one is a self-contained piece of UI. Components connect through **props** (the studs on top) and compose together to build the full application.

---

## Setting up a React + TypeScript project

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```

Vite scaffolding gives you a ready-to-go React + TypeScript project with hot module replacement.

---

## A typed component

```typescript
// Greeting.tsx
interface GreetingProps {
  name: string;
  age?: number;
}

function Greeting({ name, age }: GreetingProps): JSX.Element {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {age && <p>You are {age} years old.</p>}
    </div>
  );
}

export default Greeting;
```

Props are defined as an interface. The component destructures them from the props object. TypeScript checks that callers pass the right props:

```typescript
<Greeting name="Alice" age={30} /> // ✅
<Greeting name="Bob" />            // ✅ (age is optional)
<Greeting name={42} />             // ❌ Type 'number' is not assignable to type 'string'
```

---

## useState with types

`useState` infers the type from the initial value:

```typescript
function Counter(): JSX.Element {
  const [count, setCount] = useState(0); // type: number
  const [name, setName] = useState("");  // type: string
  const [user, setUser] = useState<User | null>(null); // explicit union

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

For complex initial state, provide the type explicitly:

```typescript
interface User { name: string; email: string; }

const [user, setUser] = useState<User | null>(null);
// setUser({ name: "Alice", email: "a@b.com" }) ✅
// setUser("hello") ❌
```

---

## Event handlers

```typescript
function Form(): JSX.Element {
  const [input, setInput] = useState("");

  function handleChange(e: React.ChangeEvent<HTMLInputElement>): void {
    setInput(e.target.value);
  }

  function handleSubmit(e: React.FormEvent<HTMLFormElement>): void {
    e.preventDefault();
    console.log("Submitted:", input);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={input} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

Common event types: `React.ChangeEvent<HTMLInputElement>`, `React.FormEvent<HTMLFormElement>`, `React.MouseEvent<HTMLButtonElement>`, `React.KeyboardEvent<HTMLInputElement>`.

---

## Children prop

```typescript
interface CardProps {
  title: string;
  children: React.ReactNode; // any renderable content
}

function Card({ title, children }: CardProps): JSX.Element {
  return (
    <div className="card">
      <h2>{title}</h2>
      {children}
    </div>
  );
}

// Usage:
<Card title="Welcome">
  <p>This is the card body.</p>
</Card>
```

---

## Component patterns: controlled inputs

```typescript
interface InputProps {
  label: string;
  value: string;
  onChange: (value: string) => void;
}

function TextInput({ label, value, onChange }: InputProps): JSX.Element {
  return (
    <div>
      <label>{label}</label>
      <input
        type="text"
        value={value}
        onChange={(e) => onChange(e.target.value)}
      />
    </div>
  );
}
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Props interface | `interface Props { name: string }` |
| Destructure props | `function Comp({ name }: Props)` |
| Optional prop | `name?: string` |
| Children | `children: React.ReactNode` |
| useState | `const [x, setX] = useState<T>(init)` |
| Event handler | `(e: React.ChangeEvent<HTMLInputElement>) => void` |

Your turn. Open DAY26/DRILL.md.
