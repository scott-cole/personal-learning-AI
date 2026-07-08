# DAY02: Drill — "Function Chef"

Write a TypeScript program in `DAY02/index.ts` that defines and calls several functions.

## Requirements

```typescript
// 1. Write a function called `toFahrenheit` that converts Celsius to Fahrenheit
//    Formula: (celsius * 9/5) + 32
//    Return type: number

// 2. Write a function called `describeTemperature` that takes a Celsius temp
//    and returns a string: "X°C is Y°F — that's [hot|cold|mild]!"
//    Use toFahrenheit inside it
//    Hot if > 30°C, cold if < 10°C, mild otherwise

// 3. Call both functions with at least 3 different values
//    Print each result with console.log
```

## Example output

```
0°C is 32°F — that's cold!
20°C is 68°F — that's mild!
35°C is 95°F — that's hot!
```

## Hints

<details>
<summary>Click for hints</summary>

- `toFahrenheit` is a pure function (input → output, no console.log inside)
- `describeTemperature` calls `toFahrenheit` for the conversion
- Use `if/else if/else` for the temperature range check
- Template literals (backticks) make string formatting easier
</details>

## Bonus

Add a third function `convertMany` that takes an array of Celsius temps and returns an array of descriptions. Use a for loop or `.map()`.

## How to run

```bash
cd ~/Dev/learn-ts/DAY02
npx tsx index.ts
```

## Before you ask for review

- Does it run without errors?
- Do the Fahrenheit conversions look correct?
- Are all function parameters and return types annotated?

## When you're done

Show me your `index.ts` file.
