# DAY22: File I/O (std::fs, Reading/Writing)

---

## The anecdote: "File I/O is a librarian checking books in and out"

A librarian manages a collection of books. They:
- **Check out** a book — open it and read it
- **Check in** a book — write new content and return it to the shelf
- **Check** if a book exists
- **Remove** a book from the collection

File I/O in Rust is the same: read files, write files, check if they exist, delete them.

---

## Reading a file

```rust
use std::fs;

fn main() -> std::io::Result<()> {
    let contents = fs::read_to_string("hello.txt")?;
    println!("File contents:\n{}", contents);
    Ok(())
}
```

`read_to_string` reads the entire file into a `String`. Use `?` to handle errors.

---

## Reading a file as bytes

```rust
use std::fs;

let data = fs::read("image.png")?;  // Vec<u8>
```

---

## Writing a file

```rust
use std::fs;

fs::write("output.txt", "Hello, file!")?;
```

`write` creates the file (or overwrites it) and writes the content.

---

## Appending to a file

```rust
use std::fs::OpenOptions;
use std::io::Write;

let mut file = OpenOptions::new()
    .append(true)
    .open("log.txt")?;

file.write_all(b"New log entry\n")?;
```

---

## Working with paths

```rust
use std::path::Path;

let path = Path::new("data/file.txt");

println!("Exists: {}", path.exists());
println!("Is file: {}", path.is_file());
println!("Is dir: {}", path.is_dir());
println!("Filename: {:?}", path.file_name());
println!("Extension: {:?}", path.extension());
```

---

## Creating and removing directories

```rust
use std::fs;

fs::create_dir("data")?;           // Create single directory
fs::create_dir_all("data/sub/a")?; // Create recursively (like mkdir -p)
fs::remove_dir("empty_dir")?;      // Remove empty directory
fs::remove_dir_all("data")?;       // Remove directory and contents
fs::remove_file("temp.txt")?;      // Remove a file
```

---

## Reading directory contents

```rust
use std::fs;

for entry in fs::read_dir(".")? {
    let entry = entry?;
    let path = entry.path();
    if path.is_file() {
        println!("File: {}", path.display());
    }
}
```

---

## Error handling with std::io::Result

```rust
use std::io;

fn read_file_safe(path: &str) -> Result<String, io::Error> {
    fs::read_to_string(path)
}

fn main() -> io::Result<()> {
    let content = read_file_safe("test.txt")
        .unwrap_or_else(|_| String::from("File not found"));
    println!("{}", content);
    Ok(())
}
```

---

## Summary

| Operation | Function | Description |
|-----------|----------|-------------|
| Read to string | `fs::read_to_string(path)?` | Read entire file |
| Read as bytes | `fs::read(path)?` | Read as Vec\<u8\> |
| Write | `fs::write(path, content)?` | Write (overwrite) |
| Append | `OpenOptions::new().append(true)` | Append to file |
| Check exists | `path.exists()` | Path existence |
| Create dir | `fs::create_dir(path)?` | Single directory |
| Create all dirs | `fs::create_dir_all(path)?` | Recursive |
| Remove file | `fs::remove_file(path)?` | Delete file |
| Remove dir | `fs::remove_dir_all(path)?` | Delete recursively |

Your turn. Open DAY22/DRILL.md.
