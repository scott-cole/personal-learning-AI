# DAY05 Checkpoint

## Questions

1. What is the difference between `dict.get(key)` and `dict[key]`?
2. Why can a `tuple` be used as a dictionary key but a `list` cannot?
3. What does `{1, 2, 3} | {3, 4, 5}` return?
4. How do you create an empty set (not an empty dict)?
5. What happens when you add a duplicate element to a set?

<details>
<summary>Answers</summary>

1. `get()` returns `None` (or a default) if the key is missing; `dict[key]` raises `KeyError`.
2. Tuples are immutable (hashable); lists are mutable (unhashable). Dictionary keys must be hashable.
3. `{1, 2, 3, 4, 5}` — union of two sets.
4. `my_set = set()` — `{}` creates an empty dict.
5. Nothing — sets silently ignore duplicates.

</details>
