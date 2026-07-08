# DAY12: Drill — "Safe Calculator with Result"

Write a TypeScript program in `DAY12/index.ts`.

## Requirements

```typescript
type Result<T> = { ok: true; value: T } | { ok: false; error: string };

// 1. Write safeDivide — returns Result<number>
// 2. Write safeParseInt(s: string): Result<number> — returns ok if parseInt succeeds
// 3. Write safeSqrt(n: number): Result<number> — error if negative
// 4. Write calculate(a: string, b: string, op: string): void
//    - Parses both strings to numbers safely
//    - Performs the operation safely
//    - Prints the result or an error message
//    - Handles: "add", "subtract", "multiply", "divide"
```

## Example output

```
> calculate("10", "5", "divide")
→ 10 / 5 = 2

> calculate("10", "0", "divide")
→ Error: Division by zero

> calculate("abc", "5", "add")
→ Error: Cannot parse "abc" as a number

> calculate("16", "", "sqrt")
→ Error: sqrt requires a non-negative number

> calculate("9", "4", "multiply")
→ 9 * 4 = 36
```

## Hints

<details>
<summary>Click for hints</summary>

- `isNaN(Number(s))` can detect parse failures, but `parseInt` returns `NaN` on failure
- `Number(s)` returns `NaN` if the string isn't a valid number
- Use `isNaN()` to check — `NaN !== NaN` in JavaScript (yes, NaN is not equal to itself)
- For `safeSqrt`, use `Math.sqrt(n)` only if `n >= 0`
- Chain safely: try to parse, if ok then try to compute
</details>

## Bonus

Add a `safeJSONParse(s: string): Result<unknown>` that wraps `JSON.parse` in try/catch and returns a Result.

## How to run

```bash
cd ~/Dev/learn-ts/DAY12
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
