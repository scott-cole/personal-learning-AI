# DAY17: Drill — "Conditional Type Utilities"

Write a TypeScript program in `DAY17/index.ts`. This drill is type-level — focus on defining the conditional types correctly and writing small functions that demonstrate them.

## Requirements

```typescript
// 1. Define a type IsArray<T> that returns true if T is an array, false otherwise
//    type Test1 = IsArray<string[]>  // true
//    type Test2 = IsArray<number>    // false

// 2. Define a type DeepReadonly<T> that recursively makes all properties readonly
//    Hint: T extends object ? { readonly [K in keyof T]: DeepReadonly<T[K]> } : T

// 3. Define a type NonNullable2<T> that removes null and undefined from T
//    (don't use the built-in NonNullable)
//    type Test3 = NonNullable2<string | null | undefined>  // string

// 4. Define a type FunctionPropertyNames<T> that returns keys whose values are functions
//    type Test4 = FunctionPropertyNames<{ foo: string; bar(): void }>  // "bar"

// 5. Write a function `isArray` that uses the conditional type to narrow at runtime
//    function isArray<T>(value: T): IsArray<T> extends true ? true : false {
//      return Array.isArray(value) as any;
//    }
```

## Example output (runtime tests)

```
isArray([1, 2, 3]) → true
isArray("hello") → false
isArray(null) → false

DeepReadonly test:
  const original = { a: { b: { c: 1 } } };
  const readonly = original as DeepReadonly<typeof original>;
  // readonly.a.b.c = 2;  // ❌ Would error
```

## Hints

<details>
<summary>Click for hints</summary>

- For `IsArray<T>`, use `T extends unknown[] ? true : false`
- `DeepReadonly` needs recursion — check `T extends object` (but exclude `Function`)
- `FunctionPropertyNames` uses `T[K] extends Function ? K : never` with key remapping
- The `as any` cast in the `isArray` function is needed because TypeScript can't evaluate the conditional type at runtime
- Test your types with `type Test = ...` and hover over them in the editor
</details>

## Bonus

Create a `DeepPartial<T>` that recursively makes all properties optional. Create a `PickByType<T, U>` that picks only properties of type `U`.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY17
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
