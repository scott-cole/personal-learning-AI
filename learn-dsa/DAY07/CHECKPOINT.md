# CHECKPOINT — Project: Array Utilities

1. What command runs all tests in verbose mode?
   <details>
   `go test -v` (or `go test ./...` for all packages).
   </details>

2. What is a table-driven test?
   <details>
   A test that uses a slice of structs (test cases) with fields like `name`, `input`, `want`, iterated in a loop.
   </details>

3. Why should you test edge cases like empty slices?
   <details>
   Because functions often crash on nil/empty input — tests catch these early.
   </details>

4. In the `Rotate` function, why is it important to handle `k > len(nums)`?
   <details>
   Rotating by k is the same as rotating by k % n. Without modulo, the triple-reverse would panic from slicing beyond bounds.
   </details>

5. What makes `RangeSum` O(1) after O(n) preprocessing?
   <details>
   The prefix sum is precomputed. A range query is just two array accesses and a subtraction, regardless of the range size.
   </details>
