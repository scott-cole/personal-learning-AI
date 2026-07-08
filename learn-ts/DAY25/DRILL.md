# DAY25: Drill — "Test-Driven Development"

Write tests first, then implement the functions. This is TDD: Red → Green → Refactor.

## Setup

```bash
cd ~/Dev/learn-ts/DAY25
npm init -y
npm install -D vitest typescript @types/node
```

## File structure

```
DAY25/
  package.json
  tsconfig.json
  src/
    utils.ts
    utils.test.ts
```

## Requirements

### Write the tests FIRST — then implement

Create `utils.ts` with these functions. Write `utils.test.ts` before writing the implementations.

### 1. `capitalize(str: string): string`
```typescript
// Tests:
it("capitalizes the first letter", () => {
  expect(capitalize("hello")).toBe("Hello");
});
it("handles empty string", () => {
  expect(capitalize("")).toBe("");
});
it("handles single character", () => {
  expect(capitalize("a")).toBe("A");
});
```

### 2. `reverse(str: string): string`
```typescript
// Tests:
it("reverses a string", () => {
  expect(reverse("hello")).toBe("olleh");
});
it("handles empty string", () => {
  expect(reverse("")).toBe("");
});
it("handles palindrome", () => {
  expect(reverse("racecar")).toBe("racecar");
});
```

### 3. `fibonacci(n: number): number[]`
```typescript
// Tests:
it("returns first n fibonacci numbers", () => {
  expect(fibonacci(5)).toEqual([0, 1, 1, 2, 3]);
});
it("returns empty for 0", () => {
  expect(fibonacci(0)).toEqual([]);
});
it("returns [0] for 1", () => {
  expect(fibonacci(1)).toEqual([0]);
});
```

### 4. `isPalindrome(str: string): boolean`
```typescript
// Tests:
it("detects a palindrome", () => {
  expect(isPalindrome("racecar")).toBe(true);
});
it("rejects non-palindrome", () => {
  expect(isPalindrome("hello")).toBe(false);
});
it("handles empty string", () => {
  expect(isPalindrome("")).toBe(true);
});
```

## Running

```bash
npx vitest run
```

## Expected output

```
✓ src/utils.test.ts (8 tests) 12ms

Tests  8 passed (8)
  Time: 1.2s
```

## Hints

<details>
<summary>Click for hints</summary>

- Write the test file FIRST, run it (it will fail — red phase)
- Then implement the functions and run again (green phase)
- `capitalize`: `str.charAt(0).toUpperCase() + str.slice(1)`
- `reverse`: `str.split("").reverse().join("")`
- `fibonacci`: loop starting with `[0, 1]`, push sum of last two
- `isPalindrome`: `str === str.split("").reverse().join("")`
</details>

## Bonus

Add property-based tests using `vitest`'s `expect` with `.rejects` for async error handling. Test a `safeDivide` function that returns a Result type.

## When you're done

Show me the terminal output of `npx vitest run` with all tests passing.
