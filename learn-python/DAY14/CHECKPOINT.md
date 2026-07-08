# DAY14 Checkpoint

## Questions

1. What does `csv.DictReader` return for each row?
2. Why does method chaining require returning `self`?
3. What is the purpose of `newline=""` when writing CSV files?
4. How would you handle numeric comparison in a filter when CSV values are strings?
5. What does the `select()` method do in the processor?

<details>
<summary>Answers</summary>

1. An `OrderedDict` mapping column headers to cell values.
2. So you can call another method on the result of the previous call: `obj.method1().method2()`.
3. It prevents the CSV writer from adding extra blank lines on Windows (related to how `\r\n` is handled).
4. Convert the string value to `float()` (or `int()`) before comparing.
5. It keeps only the specified columns by creating new dicts with just those keys.

</details>
