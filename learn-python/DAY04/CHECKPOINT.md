# DAY04 Checkpoint

## Questions

1. What is the result of `[1, 2, 3, 4, 5][::-2]`?
2. What does `fruits = ["apple", "banana"]; fruits.append("cherry"); fruits.insert(0, "date"); print(fruits)` output?
3. What is the difference between `list.pop()` and `list.remove(x)`?
4. Why does `a = [1, 2, 3]; b = a; b[0] = 99` also change `a[0]`?
5. What does `min([5, 2, 8, 1])` return?

<details>
<summary>Answers</summary>

1. `[5, 3, 1]` — starts from the end, steps back by 2 each time.
2. `['date', 'apple', 'banana', 'cherry']`
3. `pop()` removes by index (default last) and returns the value; `remove()` removes by value (first match) and returns `None`.
4. Assignment `b = a` creates an alias — both names refer to the same list object in memory.
5. `1`

</details>
