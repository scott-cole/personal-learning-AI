# DAY05: Drill — "Shape Sorter"

Write a TypeScript program in `DAY05/index.ts`.

## Requirements

Define these types:

```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number }
  | { kind: "triangle"; base: number; height: number };
```

Then:

```typescript
// 1. Write a function `area(shape: Shape): number`
//    - circle: π * r²
//    - rectangle: width * height
//    - triangle: (base * height) / 2
// 2. Create an array of at least 4 different shapes
// 3. Print the area of each shape
// 4. Write a function `describe(shape: Shape): string` that returns
//    a human-readable description like "A circle with radius 5 has area 78.54"
```

## Example output

```
A circle with radius 5 has area 78.54
A rectangle 4x6 has area 24
A triangle with base 8 and height 3 has area 12
A circle with radius 10 has area 314.16
```

## Hints

<details>
<summary>Click for hints</summary>

- Use a `switch` on `shape.kind` to narrow the type
- In each `case`, TypeScript knows the exact shape (this is discriminated union narrowing)
- `Math.PI` is available for π
- Use `area.toFixed(2)` to round to 2 decimal places
</details>

## Bonus

Add a fourth shape kind: `"square"` with a `sideLength`. Update `area` and `describe`.

## How to run

```bash
cd ~/Dev/learn-ts/DAY05
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
