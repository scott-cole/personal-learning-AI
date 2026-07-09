# DAY06: Drill — "Library System"

Write a TypeScript program in `DAY06/index.ts`.

## Requirements

```typescript
// 1. Create a base class `Item` with:
//    - public title: string
//    - public year: number
//    - private id: string (generate with Math.random().toString(36).slice(2, 10))
//    - protected isCheckedOut: boolean (default false)
//    - public method getId(): string
//    - public method checkOut(): void (sets isCheckedOut = true)
//    - public method checkIn(): void (sets isCheckedOut = false)

// 2. Create a subclass `Book` that extends Item with:
//    - public author: string
//    - public pages: number
//    - Override a method getInfo(): string returning a summary

// 3. Create a subclass `DVD` that extends Item with:
//    - public director: string
//    - public duration: number (minutes)
//    - Override getInfo(): string

// 4. Create instances, check them out/in, and print their info
```

## Example output

```
Book: "The Great Gatsby" by F. Scott Fitzgerald (1925) — 180 pages [Available]
Checked out...
Book: "The Great Gatsby" by F. Scott Fitzgerald (1925) — 180 pages [Checked out]
DVD: "Inception" directed by Christopher Nolan (2010) — 148 min [Available]
```

## Hints

<details>
<summary>Click for hints</summary>

- Use `super(title, year)` in the child constructors
- `getInfo` can be a method that returns a formatted string
- Check the `isCheckedOut` status via a public method or property
- Use constructor shorthand for all public params
</details>

## Bonus

Add a `Magazine` subclass with `issueNumber: number`. Create an array of `Item[]` and loop through calling `getInfo()`.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY06
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
