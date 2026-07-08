# DAY04: Arrays, Tuples, and the Spread Operator

---

## "An array is a numbered locker bank"

At a gym, there's a row of lockers. Each locker has a number — 0, 1, 2, 3. You open locker 0, shove your bag in. You open locker 1, put your shoes in. Each slot holds one item.

An array is exactly that. A numbered list of values, all the same type.

```typescript
let scores: number[] = [90, 85, 88, 92];
//                      [0]  [1]  [2]  [3]

console.log(scores[0]); // 90
console.log(scores[2]); // 88
```

You declare arrays with `type[]`:

```typescript
let names: string[] = ["Alice", "Bob", "Charlie"];
let mixed: (string | number)[] = ["hello", 42, "world", 7]; // union inside array
```

---

## Array methods — the tools you reach for daily

```typescript
let fruits: string[] = ["apple", "banana", "cherry"];

fruits.push("date");       // add to end:    ["apple", "banana", "cherry", "date"]
fruits.pop();              // remove from end
fruits.unshift("apricot"); // add to start
fruits.shift();            // remove from start

let index = fruits.indexOf("banana"); // 1
let hasApple = fruits.includes("apple"); // true

let sliced = fruits.slice(1, 3); // ["banana", "cherry"] (index 1 to 3, exclusive end)
```

---

## map, filter, reduce — the power trio

```typescript
const numbers: number[] = [1, 2, 3, 4, 5];

const doubled = numbers.map(n => n * 2);      // [2, 4, 6, 8, 10]
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15
```

- `map` transforms every element
- `filter` keeps elements that pass a test
- `reduce` boils the array down to a single value

These are declarative — you say *what* to do, not *how* to loop.

---

## Tuples — fixed-size lockers with mixed types

A tuple is an array with a fixed length and a specific type at each position. Think of it as a labeled pair or triplet:

```typescript
let person: [string, number] = ["Alice", 30];
//              name        age

console.log(person[0]); // "Alice"
console.log(person[1]); // 30

person[0] = "Bob"; // ✅ allowed
person[2] = true;  // ❌ Tuple type '[string, number]' has no element at index 2
```

Common use: returning multiple values from a function:

```typescript
function splitName(fullName: string): [string, string] {
  const parts = fullName.split(" ");
  return [parts[0], parts[1]];
}

const [first, last] = splitName("Scott Dev");
console.log(first); // "Scott"
console.log(last);  // "Dev"
```

---

## Spread operator — clone and merge

The spread operator `...` unpacks an array into its elements:

```typescript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const merged = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]

const copy = [...arr1]; // shallow copy — safe for primitives
arr1.push(99);
console.log(copy); // [1, 2, 3] — original unchanged
```

Spread is non-destructive. You create new arrays instead of mutating old ones. This is the functional programming habit you want to build early.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Array type | `let arr: number[]` or `Array<number>` |
| Access element | `arr[0]` |
| Add to end | `arr.push(x)` |
| Transform | `arr.map(fn)` |
| Filter | `arr.filter(fn)` |
| Reduce | `arr.reduce(fn, init)` |
| Tuple | `let t: [string, number]` |
| Spread | `[...arr1, ...arr2]` |

Your turn. Open DAY04/DRILL.md.
