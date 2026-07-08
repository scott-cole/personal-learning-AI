# DAY03: Drill — "Build a Library Catalog"

Write a TypeScript program in `DAY03/index.ts`.

## Requirements

Define these interfaces:

```typescript
interface Book {
  title: string;
  author: string;
  pages: number;
  isbn?: string; // optional
}

interface Library {
  name: string;
  address: string;
  books: Book[]; // array of books
  openHours?: string; // optional
}
```

Then:

```typescript
// 1. Create a Library object with at least 3 books
// 2. Write a function `findBookByTitle(library: Library, title: string): Book | undefined`
// 3. Write a function `totalPages(library: Library): number` that sums all book pages
// 4. Call both functions and print the results
```

## Example output

```
Library: Central Library
Books:
- The Great Gatsby (180 pages)
- 1984 (328 pages)
- To Kill a Mockingbird (281 pages)
Total pages: 789
Searching for "1984": found — 1984 by George Orwell (328 pages)
Searching for "Nonexistent": not found
```

## Hints

<details>
<summary>Click for hints</summary>

- `Book[]` means "array of Book objects"
- Use `.find()` on the array for `findBookByTitle`
- Use `.reduce()` or a for loop for `totalPages`
- Return `undefined` from `findBookByTitle` when no match — that's what the `| undefined` in the return type means
</details>

## Bonus

Add a `genre` property to `Book` and a function `booksByGenre(library: Library, genre: string): Book[]`.

## How to run

```bash
cd ~/Dev/learn-ts/DAY03
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
