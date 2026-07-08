# CHECKPOINT — Stacks

1. What does LIFO stand for?
   <details>
   Last In, First Out — the most recently added item is removed first.
   </details>

2. What is the time complexity of push and pop?
   <details>
   Both are O(1) — push appends, pop removes from the end.
   </details>

3. Give an example of a real-world stack.
   <details>
   The call stack in a program (function calls), or the undo stack in an editor.
   </details>

4. In the balanced parentheses problem, what invalid case does checking `len(stack) == 0` on pop catch?
   <details>
   A closing bracket with no matching opening bracket, like `")"`.
   </details>

5. What is `peek` and how is it different from `pop`?
   <details>
   `peek` returns the top value without removing it. `pop` removes it.
   </details>
