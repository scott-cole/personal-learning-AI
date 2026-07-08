# DAY10: Drill — "Modular Calculator"

Write a multi-file TypeScript program in `DAY10/`.

## File structure

```
DAY10/
  index.ts      — entry point, imports everything
  operations.ts — math functions
  display.ts    — formatting functions
  types.ts      — shared types
```

## Requirements

### types.ts
```typescript
export type Operator = "add" | "subtract" | "multiply" | "divide";

export interface Calculation {
  a: number;
  b: number;
  operator: Operator;
  result: number;
}
```

### operations.ts
```typescript
export function add(a: number, b: number): number;
export function subtract(a: number, b: number): number;
export function multiply(a: number, b: number): number;
export function divide(a: number, b: number): number; // handle division by zero → throw
export function calculate(a: number, b: number, op: Operator): number; // switch on op
```

### display.ts
```typescript
// Default export — a function that formats a Calculation as a string
export default function formatCalc(calc: Calculation): string;

// Named export — prints a history of calculations
export function printHistory(history: Calculation[]): void;
```

### index.ts
```typescript
// Import from each module
// Perform at least 4 calculations (add, subtract, multiply, divide)
// Store them in a history array
// Print each result
// Print the full history at the end
```

## Example output

```
5 + 3 = 8
10 - 4 = 6
6 * 7 = 42
20 / 4 = 5

History:
1. 5 + 3 = 8
2. 10 - 4 = 6
3. 6 * 7 = 42
4. 20 / 4 = 5
```

## Hints

<details>
<summary>Click for hints</summary>

- `divide` should `throw new Error("Division by zero")` if b === 0
- Use `export default function` for `formatCalc`
- `import calcFormatter, { printHistory } from "./display"`
- Catch the division-by-zero error with try/catch in index.ts
</details>

## Bonus

Add a `scientific.ts` module with `power` and `sqrt` functions. Export and use them from index.ts.

## How to run

```bash
cd ~/Dev/learn-ts/DAY10
npx tsx index.ts
```

## When you're done

Show me all four files.
