# DAY26: Drill — "Async Web Scraper"

Create a new Cargo project:

```bash
cargo new --bin async_scraper
cd async_scraper
```

Add to `Cargo.toml`:

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
reqwest = { version = "0.12", features = ["json"] }
```

## Requirements

Build an async program that fetches multiple URLs concurrently.

```rust
use std::time::Instant;

// TODO: Write an async function that fetches a URL and returns
// the status code and content length
async fn fetch_url(url: &str) -> Result<(u16, usize), reqwest::Error> {
    // TODO: use reqwest::get(url).await?
    // Then get .status().as_u16() and .text().await?.len()
}

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let start = Instant::now();

    let urls = vec![
        "https://httpbin.org/delay/1",
        "https://httpbin.org/delay/2",
        "https://httpbin.org/delay/3",
    ];

    // TODO: Fetch all URLs CONCURRENTLY using tokio::join!
    // or by collecting futures into a Vec and using futures::future::join_all

    // TODO: Print each result with status code and size

    let duration = start.elapsed();
    println!("\nTotal time: {:?}", duration);
    // If sequential: ~6 seconds
    // If concurrent: ~3 seconds!

    Ok(())
}
```

## Example output

```
https://httpbin.org/delay/1 → 200 (312 bytes)
https://httpbin.org/delay/2 → 200 (312 bytes)
https://httpbin.org/delay/3 → 200 (312 bytes)

Total time: 3.01s
```

## Hints

<details>
<summary>Click for hints</summary>

- `reqwest::get(url).await?` returns a Response
- `response.status().as_u16()` for status code
- `response.text().await?.len()` for content length
- For concurrent execution, collect futures in a Vec and use `futures::future::join_all(futures).await`
- Or add `futures` crate to Cargo.toml
- Alternatively, use `tokio::join!(url1, url2, url3)` for fixed number of URLs
</details>

## Bonus

Make the URLs configurable via command-line arguments using `std::env::args()`.

## How to run

```bash
cd ~/Dev/learn-rust/DAY26/async_scraper
cargo run
```

## Before you ask for review

- Does it compile?
- Is the total time ~3 seconds (not 6)? If it's 6, the fetches are sequential, not concurrent
- Are all status codes 200?
- What happens if a URL is unreachable?

## When you're done

Show me your `src/main.rs` and the output.
