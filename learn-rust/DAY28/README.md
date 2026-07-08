# DAY28: CLI App with clap

---

## The anecdote: "clap is a waiter taking your order"

You sit at a restaurant. The waiter says "What would you like?" You say "I'd like the steak, medium rare, with a side of mashed potatoes and a glass of Cabernet."

The waiter listens, confirms your order, and sends it to the kitchen.

clap does the same for command-line programs. It listens to command-line arguments, validates them, and gives you structured data.

```bash
# Without clap — manual parsing:
./app --name Scott --age 30

# With clap — structured, validated, with help text:
./app --name Scott --age 30
```

---

## Adding clap

```toml
[dependencies]
clap = { version = "4", features = ["derive"] }
```

---

## Derive approach (recommended)

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "greeter")]
#[command(about = "A friendly greeter", version = "1.0")]
struct Cli {
    /// Name of the person to greet
    name: String,

    /// Number of times to greet
    #[arg(short = 'n', long = "times", default_value = "1")]
    times: u32,

    /// Be extra polite
    #[arg(short, long)]
    polite: bool,
}

fn main() {
    let args = Cli::parse();

    for _ in 0..args.times {
        if args.polite {
            println!("Hello, {}! It's a pleasure to meet you!", args.name);
        } else {
            println!("Hi {}", args.name);
        }
    }
}
```

---

## Running the CLI

```bash
cargo run -- Scott
# Hi Scott

cargo run -- Scott -n 3
# Hi Scott
# Hi Scott
# Hi Scott

cargo run -- Scott -n 2 --polite
# Hello, Scott! It's a pleasure to meet you!
# Hello, Scott! It's a pleasure to meet you!

cargo run -- --help
# A friendly greeter
#
# Usage: greeter <NAME> [OPTIONS]
#
# Arguments:
#   <NAME>                 Name of the person to greet
#
# Options:
#   -n, --times <TIMES>    Number of times to greet [default: 1]
#   -p, --polite           Be extra polite
#   -h, --help             Print help
#   -V, --version          Print version
```

---

## Subcommands

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
struct Cli {
    #[command(subcommand)]
    command: Command,
}

#[derive(Subcommand)]
enum Command {
    /// Add a task
    Add { task: String },
    /// List all tasks
    List,
    /// Remove a task
    Remove { id: u32 },
}

fn main() {
    let args = Cli::parse();

    match args.command {
        Command::Add { task } => println!("Adding: {}", task),
        Command::List => println!("Listing all tasks"),
        Command::Remove { id } => println!("Removing task {}", id),
    }
}
```

---

## Summary

| Concept | Code | Description |
|---------|------|-------------|
| Define CLI | `#[derive(Parser)] struct Cli` | Main struct |
| Positional arg | `name: String` | Required argument |
| Named option | `#[arg(short, long)]` | `--flag value` |
| Flag | `#[arg(short, long)]` bool field | `--flag` |
| Default value | `#[arg(default_value = "x")]` | Default when omitted |
| Help text | `/// doc comment` | Auto-generated help |
| Subcommand | `#[derive(Subcommand)] enum Cmd` | Nested commands |

Your turn. Open DAY28/DRILL.md.
