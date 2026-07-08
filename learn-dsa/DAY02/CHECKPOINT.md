# CHECKPOINT — Arrays and Slices

1. What is the zero value of `var a [5]int`?
   <details>
   `[0 0 0 0 0]` — all elements are zero-valued.
   </details>

2. Can you append to a nil slice?
   <details>
   Yes. `append(nil, 1)` works — Go allocates a backing array automatically.
   </details>

3. What does `s[1:4]` give if `s := []int{10, 20, 30, 40, 50}`?
   <details>
   `[20 30 40]`. The start index is inclusive, end is exclusive.
   </details>

4. What is the difference between `len` and `cap`?
   <details>
   `len` is the number of elements; `cap` is the size of the backing array.
   </details>

5. How does a slice grow when it exceeds capacity?
   <details>
   Go allocates a new backing array (usually 2x the old capacity) and copies elements.
   </details>
