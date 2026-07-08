# DAY01: Drill — "How old are you, really?"

Write a TypeScript program from scratch. Create a file called `index.ts` inside the `DAY01/` folder.

## Requirements

```typescript
// 1. Store your name in a variable
// 2. Store your age in a variable
// 3. Calculate your approximate age in days (age * 365)
// 4. Print: "Hello, I'm [name] and I'm about [days] days old!"
```

## Example output

```
Hello, I'm Scott and I'm about 10950 days old!
```

## Hints (try without these first)

<details>
<summary>Click for hints</summary>

- Use `const` for your name, `let` for your age
- Age in days is `age * 365`
- `console.log` can take multiple arguments joined by a comma
- Template literals with backticks work too: `` `Hello, I'm ${name}...` ``
</details>

## Bonus (if you finish early)

Add a line that also prints your age in **hours**:

```
I'm approximately 262800 hours old!
```

And add boolean variables `isStudent` and `isEmployed` — log them too.

## How to run

```bash
cd ~/Dev/learn-ts/DAY01
npx tsx index.ts
```

## Before you ask for review

- Does it run without errors? (`npx tsx index.ts`)
- Does the output match the example?
- Try changing the values — does it still work?

## When you're done

Show me your `index.ts` file. I'll review every line.
