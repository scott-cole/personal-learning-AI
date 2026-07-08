# DAY04: Drill — "Scoreboard"

Write a TypeScript program in `DAY04/index.ts`.

## Requirements

```typescript
// 1. Create an array of scores: number[] = [85, 92, 78, 95, 88, 72, 91]
// 2. Write these functions:
//    - average(scores: number[]): number — computes the mean
//    - highest(scores: number[]): number — finds the max
//    - passing(scores: number[], threshold: number): number[] — filters scores >= threshold
//    - toLetterGrade(score: number): string — converts to "A" (>=90), "B" (>=80), "C" (>=70), "F" (<70)
// 3. Print the results

// Bonus: Return each student as a tuple [name: string, score: number]
//        and print the letter grade for each
```

## Example output

```
Scores: 85, 92, 78, 95, 88, 72, 91
Average: 85.86
Highest: 95
Passing (>= 80): 85, 92, 95, 88, 91

Letter grades:
- Alice: 85 = B
- Bob: 92 = A
- Charlie: 78 = C
- Diana: 95 = A
- Eve: 88 = B
- Frank: 72 = C
- Grace: 91 = A
```

## Hints

<details>
<summary>Click for hints</summary>

- `average` can use `reduce` to sum, then divide by `scores.length`
- `highest` can use `Math.max(...scores)` or a loop
- `toFixed(2)` rounds a number to 2 decimal places for display
- Use `.map()` to transform the array of tuples into strings
</details>

## How to run

```bash
cd ~/Dev/learn-ts/DAY04
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
