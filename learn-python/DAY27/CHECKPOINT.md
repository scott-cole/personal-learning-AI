# DAY27 Checkpoint

## Questions

1. What keyword defines a coroutine in Python?
2. What does `await` do when called on a coroutine?
3. What is the difference between running 3 tasks sequentially with `await` vs. concurrently with `asyncio.gather()`?
4. How do you add a timeout to an async operation?
5. What kind of tasks benefit most from `asyncio`?

<details>
<summary>Answers</summary>

1. `async def` — defines a coroutine function.
2. It pauses the current coroutine, yields control to the event loop, and resumes when the awaited operation completes.
3. Sequential: total time is sum of all durations. Concurrent: total time is the maximum duration (they overlap).
4. Use `asyncio.wait_for(coroutine, timeout=seconds)` which raises `asyncio.TimeoutError` if it exceeds the limit.
5. I/O-bound tasks — network requests, file I/O, database queries — where the program spends most time waiting.

</details>
