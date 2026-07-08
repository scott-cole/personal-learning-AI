# CHECKPOINT — Browser Navigation Project

1. How many stacks does the browser navigation system use?
   <details>
   Two: a back stack and a forward stack.
   </details>

2. When does the forward stack get cleared?
   <details>
   When the user visits a new page after having gone back.
   </details>

3. What happens to the current page when you click "back"?
   <details>
   It is pushed onto the forward stack, and the top of the back stack becomes the current page.
   </details>

4. In terms of algorithmic efficiency, what are the time complexities of Back() and Forward()?
   <details>
   Both are O(1) — they just push and pop from slices (amortised).
   </details>

5. How would you limit the stacks to prevent infinite memory growth?
   <details>
   Set a maximum size. When the stack exceeds it, remove the oldest (bottom) element.
   </details>
