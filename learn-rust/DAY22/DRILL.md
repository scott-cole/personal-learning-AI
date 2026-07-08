# DAY22: Drill — "File Manager"

Create `main.rs` in `DAY22/`.

## Requirements

Build a program that manages a simple text file database.

```rust
use std::fs;
use std::io;

// TODO: Write a function that creates a file with the given content
fn create_file(filename: &str, content: &str) -> io::Result<()> {
    // TODO
}

// TODO: Write a function that reads a file and returns its content
fn read_file(filename: &str) -> io::Result<String> {
    // TODO
}

// TODO: Write a function that appends a line to a file
fn append_line(filename: &str, line: &str) -> io::Result<()> {
    // TODO
}

// TODO: Write a function that lists all .txt files in a directory
fn list_txt_files(dir: &str) -> io::Result<Vec<String>> {
    // TODO
}

fn main() -> io::Result<()> {
    // 1. Create a file called "notes.txt" with content "My Notes\n"
    create_file("notes.txt", "My Notes\n")?;

    // 2. Append three lines
    append_line("notes.txt", "1. Buy groceries")?;
    append_line("notes.txt", "2. Learn Rust")?;
    append_line("notes.txt", "3. Build something cool")?;

    // 3. Read and print the file
    let content = read_file("notes.txt")?;
    println!("=== notes.txt ===");
    println!("{}", content);

    // 4. List all .txt files in current directory
    let txt_files = list_txt_files(".")?;
    println!("=== .txt files ===");
    for f in txt_files {
        println!("  {}", f);
    }

    // 5. Create a backup copy
    fs::copy("notes.txt", "notes_backup.txt")?;
    println!("Backup created!");

    Ok(())
}
```

## Example output

```
=== notes.txt ===
My Notes
1. Buy groceries
2. Learn Rust
3. Build something cool

=== .txt files ===
  notes.txt

Backup created!
```

## Hints

<details>
<summary>Click for hints</summary>

- `create_file`: `fs::write(filename, content)`
- `read_file`: `fs::read_to_string(filename)`
- `append_line`: `use std::io::Write;` then `OpenOptions::new().append(true).open(filename)?.write_all(format!("{}\n", line).as_bytes())`
- `list_txt_files`: `fs::read_dir(dir)?` then `filter_map` entries that end with `.txt`
</details>

## Bonus

Add a `search_files(dir: &str, pattern: &str) -> io::Result<Vec<String>>` function that finds files containing the pattern.

## How to run

```bash
cd ~/Dev/learn-rust/DAY22
rustc main.rs && ./main
```

Clean up after: `rm notes.txt notes_backup.txt`

## Before you ask for review

- Does it compile?
- Does `notes.txt` contain the expected content?
- Does listing .txt files find the right files?
- What happens if `notes.txt` doesn't exist?

## When you're done

Show me your `main.rs`.
