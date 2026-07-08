# DAY28: Checkpoint

**Closed book.** No looking back.

1. What derive macro does clap use for CLI structs?
   Parser

2. How do you define a positional argument in clap?
   a field in the struct without any arg attribute

3. How do you define a boolean flag like `--verbose`?
   #[arg(short, long)] verbose: bool

4. What derive macro is used for subcommands?
   Subcommand (on an enum)

5. What does `cargo run -- --help` do differently from `cargo run -- add --help`?
   shows help for the main CLI vs help for the add subcommand

---

**Check your answers against the lesson after you finish.**
