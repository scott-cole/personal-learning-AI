# DAY27 — Async/Await with asyncio

## Anecdote: Async is a juggler keeping multiple balls in the air

A chef in a kitchen can only chop one onion at a time (synchronous). While chopping, the stove boils water, but the chef ignores it until the onion is done. That's wasteful — the stove could be working in parallel.

Now watch a juggler. They toss ball A, then while it's in the air, they toss ball B, then C. While the balls are airborne, the juggler isn't waiting — they're already reaching for the next ball. Ball A lands, they catch and rethrow. Nothing waits. The juggler's hands are never idle.

`asyncio` is that juggler. An `async` function is a ball in the air. `await` is the moment the juggler catches a ball. While one task awaits (e.g., waiting for a network response), the event loop picks up another task and runs it. The CPU is never idle.

---

## The event loop

`asyncio` runs an **event loop** that schedules and runs multiple coroutines concurrently.

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)  # Simulate waiting (network, I/O)
    print("World")

asyncio.run(say_hello())
```

`asyncio.run()` creates the event loop, runs the coroutine, and closes the loop.

---

## `async` and `await`

- `async def` — defines a coroutine (a function that can be paused and resumed)
- `await` — yields control back to the event loop until the awaited operation finishes

```python
async def fetch_data(url):
    print(f"Fetching {url}...")
    await asyncio.sleep(2)  # Simulate network latency
    return f"Data from {url}"

async def main():
    result = await fetch_data("https://api.example.com")
    print(result)

asyncio.run(main())
```

---

## Running multiple tasks concurrently

```python
async def fetch_data(url, delay):
    print(f"Fetching {url}...")
    await asyncio.sleep(delay)
    return f"Data from {url}"

async def main():
    # Run tasks concurrently
    results = await asyncio.gather(
        fetch_data("url1", 3),
        fetch_data("url2", 2),
        fetch_data("url3", 1),
    )
    print(results)  # All done after ~3s, not 6s

asyncio.run(main())
```

Without `gather`, the total time would be 3 + 2 + 1 = 6 seconds. With `gather`, it's ~3 seconds (the longest single task).

---

## `create_task` — fire and forget

```python
async def main():
    # Start the task in the background
    task = asyncio.create_task(fetch_data("url", 3))

    # Do other work while the task runs
    print("Doing other work...")
    await asyncio.sleep(1)

    # Wait for the task to finish
    result = await task
    print(result)
```

---

## Async HTTP requests with `aiohttp`

```bash
pip install aiohttp
```

```python
import aiohttp
import asyncio

async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()

async def main():
    async with aiohttp.ClientSession() as session:
        html = await fetch(session, "https://example.com")
        print(f"Got {len(html)} bytes")

asyncio.run(main())
```

### Concurrent HTTP requests

```python
async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in urls]
        return await asyncio.gather(*tasks)
```

---

## Async file I/O

Standard file operations block the event loop. Use `aiofiles` for non-blocking file I/O.

```bash
pip install aiofiles
```

```python
import aiofiles
import asyncio

async def read_file():
    async with aiofiles.open("data.txt", "r") as f:
        content = await f.read()
    return content
```

---

## Timeouts and cancellation

```python
async def main():
    try:
        result = await asyncio.wait_for(
            fetch_data("slow-url", 10),
            timeout=3.0
        )
    except asyncio.TimeoutError:
        print("Request timed out!")
```

---

## When to use async

| Use async | Don't use async |
|-----------|-----------------|
| Network requests (HTTP, DB) | CPU-bound calculations |
| File I/O (with aiofiles) | Simple scripts |
| Web servers | Data processing |
| Multiple I/O-bound tasks | Math/algorithm-heavy code |

For CPU-bound work, use `multiprocessing` or threads instead.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `async def` | Define a coroutine |
| `await` | Pause until the awaited task completes |
| `asyncio.run()` | Start the event loop |
| `asyncio.gather()` | Run multiple coroutines concurrently |
| `asyncio.create_task()` | Schedule a coroutine in the background |
| `asyncio.wait_for()` | Add a timeout |
| `aiohttp` | Async HTTP client |
| Ideal for | I/O-bound concurrent operations |
