# Personal Learning AI

Structured, anecdote-based daily curricula for learning software engineering from zero to production-ready. Each curriculum follows the same format: **READ ME** (lesson with a visual anecdote) → **DRILL** (hands-on coding task) → **CHECKPOINT** (quiz to reinforce). No copy-paste — you build everything from scratch.

## Curricula

| Curriculum | Days | Language/Tool | Covers |
|------------|------|---------------|--------|
| [learn-go](./learn-go/) | 28 | Go | Variables → functions → structs → slices → maps → pointers → HTTP → services → middleware → SQL → testing → concurrency → production |
| [learn-ts](./learn-ts/) | 30 | TypeScript | Types → interfaces → generics → async → DOM → Express → Zod → React → full-stack project |
| [learn-rust](./learn-rust/) | 30 | Rust | Ownership → borrowing → lifetimes → traits → closures → concurrency → serde → axum → async → URL shortener |
| [learn-web](./learn-web/) | 30 | curl / Go | HTTP methods → status codes → REST → JWT → CORS → DNS → TLS → WebSockets → rate limiting → proxies → CDN |
| [learn-sql](./learn-sql/) | 30 | PostgreSQL | SELECT → joins → aggregates → subqueries → indexes → transactions → views → normalization → window functions → CTEs → EXPLAIN |
| [learn-python](./learn-python/) | 30 | Python 3 | Functions → lists → dicts → comprehensions → decorators → generators → Flask → SQLite → pytest → asyncio → URL shortener |
| [learn-dsa](./learn-dsa/) | 30 | Go | Big O → arrays → linked lists → stacks → queues → trees → graphs → sorting → recursion → DP → backtracking → search engine |

## Format

Every day folder contains three files:

```
DAY01/
├── README.md      # Lesson with anecdote + code examples + summary
├── DRILL.md       # Hands-on task with starter code + TODOs + expected output
└── CHECKPOINT.md  # 5 quiz questions with answers in <details>
```

## How to use

Pick a curriculum, start at DAY01, and work through sequentially:

1. **Read** the README — absorb the anecdote and concept
2. **Build** the drill — write code from scratch using the prompts
3. **Test** yourself with the checkpoint — verify you retained it

The curricula are designed to be language-agnostic where possible and build toward real production skills: HTTP APIs, databases, testing, concurrency, and deployment-ready patterns.
