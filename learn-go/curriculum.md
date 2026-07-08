# Learn Go: URL Shortener Curriculum

## The Philosophy

You're not learning Go syntax. You're learning to **build production backends**.
Go is the tool. Architecture, testing, concurrency, data — those are the skills.

Every concept connects to a real production problem. If I can't tell you why
a senior engineer needs this, I won't teach it.

---

## How each day works

1. **Read the lesson** (5 min)
2. **Do the drill** (15-45 min) — write code, run it, get it green
3. **Take the checkpoint** (3 min) — closed-book, prove it stuck
4. **Show me your code** — I review line-by-line

---

## Timeline

### Week 1: Go Brain Training (fundamentals)

| Day | Topic | Anecdote | Why a senior needs this |
|-----|-------|----------|------------------------|
| 1 | Variables, types, fmt | "Labelled boxes on a shelf" | Every line of production code starts here |
| 2 | Functions, returns, errors | "A function is a blender" | Services are just functions wired together |
| 3 | Structs and methods | "A struct is a paper form" | Your domain model (User, Order, Payment) is structs |
| 4 | Slices | "A magic expanding backpack" | Lists of data — users, transactions, logs |
| 5 | Maps and sets | "A coat check ticket" | Lookup tables, caches, indexes |
| 6 | Pointers | "A house address, not the house" | Performance, mutations, nil safety |
| 7 | Week 1 review + mini-project | — | Build a CLI tool end-to-end |

### Week 2: Layers of a Backend

| Day | Topic | Anecdote | Why a senior needs this |
|-----|-------|----------|------------------------|
| 8 | Interfaces | "A USB port" | Interface is the most important Go concept |
| 9 | Repository pattern | "A filing cabinet" | Swapping in-memory ↔ SQL without changing business logic |
| 10 | Service layer | "The manager" | Business logic lives here, not in handlers |
| 11 | HTTP handlers & routing | "The receptionist" | Every backend starts with a request |
| 12 | JSON: marshal/unmarshal | "The common language" | Every backend speaks JSON |
| 13 | Middleware pattern | "The security guard" | Logging, auth, rate limiting — cross-cutting concerns |
| 14 | URL shortener v1 | — | End-to-end working service |

### Week 3: Real Data

| Day | Topic | Why a senior needs this |
|-----|-------|------------------------|
| 15 | SQL fundamentals (SELECT, INSERT) | You can't avoid the database |
| 16 | database/sql + SQLite | Connecting Go to a real DB |
| 17 | Migrations & schema design | Your structure changes over time |
| 18 | Repository with SQL | Swap the in-memory repo for real persistence |
| 19 | Transactions | Money moves, inventory — consistency matters |
| 20 | Error wrapping and logging | "Where did this error come from?" |
| 21 | Week 3 review | Write 5 queries from memory |

### Week 4: Production Readiness

| Day | Topic | Why a senior needs this |
|-----|-------|------------------------|
| 22 | Configuration (env vars, flags) | No hardcoded values in production |
| 23 | Unit & integration testing | Tests are not optional |
| 24 | Concurrency (goroutines, mutexes) | Handling 1000 requests at once |
| 25 | Graceful shutdown | Restarting without dropping requests |
| 26 | Rate limiting | Protecting your service |
| 27 | Project structure (cmd, internal, pkg) | The standard layout |
| 28 | Final project: URL shortener with analytics | Portfolio-ready |

---

## The Senior Engineer Mindset

These aren't in a specific day — they're threaded through everything:

- **Own your errors** — never ignore a returned error
- **Favour readability over cleverness** — the next person reading your code will be you in 6 months
- **Compose, don't inherit** — Go has no classes. Use interfaces and composition.
- **Test behaviour, not implementation** — your tests should pass even if you rewrite the internals
- **Think in layers** — handler calls service calls repository. Never skip a layer.
- **Fail fast** — validate inputs at the boundary, not in the middle of business logic

---

## Progress tracker

- [x] Day 1: Variables, types, fmt
- [x] Day 2: Functions, returns, errors
- [x] Day 3: Structs and methods
- [x] Day 4: Slices
- [x] Day 5: Maps
- [x] Day 6: Pointers
- [x] Day 7: Week 1 project
- [x] Day 8: Interfaces
- [x] Day 9: Repository pattern
- [x] Day 10: Service layer
- [ ] Day 11: HTTP handlers
- [ ] Day 12: JSON
- [ ] Day 13: Middleware
- [ ] Day 14: URL shortener v1
- [ ] Day 15: SQL
- [ ] Day 16: database/sql
- [ ] Day 17: Migrations
- [ ] Day 18: SQL repository
- [ ] Day 19: Transactions
- [ ] Day 20: Error handling
- [ ] Day 21: Week 3 review
- [ ] Day 22: Configuration
- [ ] Day 23: Testing
- [ ] Day 24: Concurrency
- [ ] Day 25: Graceful shutdown
- [ ] Day 26: Rate limiting
- [ ] Day 27: Project structure
- [ ] Day 28: Final project
