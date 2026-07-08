# DAY11: Drill — "Async Kitchen"

Write a TypeScript program in `DAY11/index.ts`.

## Requirements

```typescript
// 1. Write a function `preheatOven(temp: number): Promise<string>`
//    - Waits 1 second, then resolves with "Oven preheated to X°C"

// 2. Write a function `chopVegetable(veg: string): Promise<string>`
//    - Waits 500ms, then resolves with "Chopped [veg]"

// 3. Write a function `bakePizza(): Promise<string>`
//    - Waits 2 seconds, then resolves with "Pizza is ready!"

// 4. Write an async function `cookMeal()`
//    - Preheats oven
//    - Chops veggies (at least 2: "tomatoes" and "peppers")
//    - Bakes pizza
//    - Do the chopping in PARALLEL with Promise.all
//    - Print each step as it completes
//    - Return "Meal is ready!"
```

## Example output

```
Starting to cook...
Oven preheated to 220°C
Chopped tomatoes
Chopped peppers
Pizza is ready!
Meal is ready! Total time: ~3 seconds
```

## Hints

<details>
<summary>Click for hints</summary>

- `preheatOven` should await a `new Promise` with `setTimeout`
- `Promise.all` returns an array of results in the same order
- `console.time("meal")` and `console.timeEnd("meal")` to measure elapsed time
- `await` each step sequentially for oven → veggies → pizza — but chop veggies in parallel
</details>

## Bonus

Add a `function washDishes(): Promise<string>` that takes 3 seconds and runs IN PARALLEL with the baking. Log "Washed dishes" around the same time as "Pizza is ready!".

## How to run

```bash
cd ~/Dev/learn-ts/DAY11
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
