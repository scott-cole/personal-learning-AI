# DAY27 Drill — The Juggler

## Task

Write a Python script (`drill.py`) that demonstrates async/await with asyncio.

### Requirements

1. **`simulate_task(name, duration)`** — an async function that:
   - Prints `"Task {name}: started"`
   - Awaits `asyncio.sleep(duration)`
   - Prints `"Task {name}: finished after {duration}s"`
   - Returns `"Result from {name}"`

2. **Sequential execution** — Run 3 tasks sequentially (one after another) and measure the total time

3. **Concurrent execution** — Run the same 3 tasks concurrently using `asyncio.gather()` and measure the total time

4. **With timeout** — Run a task that takes 5 seconds but with a 2-second timeout, handling the `TimeoutError`

5. **Producer-consumer pattern** — Create a simple producer that puts items into a queue and a consumer that processes them

### Expected output

```
=== SEQUENTIAL (should take ~6s) ===
Task A: started
Task A: finished after 1s
Task B: started
Task B: finished after 2s
Task C: started
Task C: finished after 3s
Sequential total time: 6.0Xs

=== CONCURRENT (should take ~3s) ===
Task X: started
Task Y: started
Task Z: started
Task Z: finished after 1s
Task Y: finished after 2s
Task X: finished after 3s
Concurrent total time: 3.0Xs

=== TIMEOUT ===
Task SLOW: started
Task timed out!

=== QUEUE (producer/consumer) ===
Producer put: item-0
Consumer got: item-0
Producer put: item-1
Consumer got: item-1
... (all 5 items)
```

### Hints

- `asyncio.gather(*tasks)` to run concurrently
- Use `asyncio.wait_for(task, timeout=2.0)` for the timeout demo
- `asyncio.Queue()` for producer-consumer
- Measure time with `time.perf_counter()` around the `asyncio.run()` call

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Define `simulate_task()` coroutine
- [ ] Run tasks sequentially with `await`
- [ ] Run tasks concurrently with `asyncio.gather()`
- [ ] Demonstrate timeout with `asyncio.wait_for`
- [ ] Implement producer-consumer with `asyncio.Queue`
- [ ] Measure and print elapsed times
