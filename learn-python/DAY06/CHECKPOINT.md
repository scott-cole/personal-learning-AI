# DAY06 Checkpoint

## Questions

1. What does `print("even" if 4 % 2 == 0 else "odd")` output?
2. What is the output of `for i in range(3): print(i, end=" ")`?
3. What does the `else` clause on a `for` loop do?
4. How do you loop through a list while also having access to the index?
5. What does `while False: print("runs")` output?

<details>
<summary>Answers</summary>

1. `"even"`
2. `0 1 2 `
3. It runs only if the loop completed without hitting a `break`.
4. Use `enumerate()` — `for i, item in enumerate(my_list):`
5. Nothing — the condition is `False`, so the body never executes.

</details>
