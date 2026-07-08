# DAY06: Structs, impl Blocks, Associated Functions

---

## The anecdote: "A struct is a paper form; impl is the instruction manual"

Imagine you're at a doctor's office. You fill out a paper form:

```
Patient Name: [        ]
Age:          [        ]
Symptoms:     [        ]
```

That form is a **struct** — it's a template that groups related data together.

```rust
struct Patient {
    name: String,
    age: i32,
    symptoms: Vec<String>,
}
```

Now imagine a laminated instruction manual next to the clipboard that tells you how to fill out the form, validate it, and print it. That instruction manual is the **impl block**.

---

## Defining a struct

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

The struct name is PascalCase (capital first letter for each word). Fields are snake_case.

---

## Creating an instance

```rust
let user = User {
    username: String::from("scott"),
    email: String::from("scott@example.com"),
    sign_in_count: 1,
    active: true,
};
```

---

## Accessing fields

```rust
println!("{}", user.email);  // dot notation
let mut user = User { /* ... */ };
user.email = String::from("new@example.com");  // mutable — require mut
```

---

## Field init shorthand

If the variable name matches the field name, you can use a shorthand:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        username,  // shorthand for username: username
        email,
        sign_in_count: 1,
        active: true,
    }
}
```

---

## impl blocks

The `impl` block is where you define functions that belong to a struct.

### Associated functions (called on the type, not an instance)

```rust
impl User {
    fn new(email: String, username: String) -> User {
        User {
            username,
            email,
            sign_in_count: 1,
            active: true,
        }
    }
}

let user = User::new("scott@example.com", "scott");
```

`User::new()` is like a constructor. `::` is used for namespace access.

### Methods (called on an instance)

```rust
impl User {
    fn full_info(&self) -> String {
        format!("{} <{}> (signed in {})",
            self.username, self.email, self.sign_in_count)
    }
}

println!("{}", user.full_info());
```

`&self` is shorthand for `self: &Self`. It borrows the struct. You can also use `&mut self` or `self` (consuming).

---

## The Tuple Struct

Sometimes you want a named type without naming each field:

```rust
struct Color(i32, i32, i32);
let black = Color(0, 0, 0);
println!("{}", black.0);  // access by position
```

---

## Summary

| Concept | Syntax |
|---------|--------|
| Define a struct | `struct Name { field: Type, }` |
| Create instance | `Name { field: value }` |
| Field shorthand | `Name { field }` for `field: field` |
| Method | `fn method(&self) { }` |
| Associated function | `fn name() -> Self { }` |
| Call associated fn | `Name::function()` |
| Call method | `instance.method()` |
| Tuple struct | `struct Name(Type1, Type2);` |

Your turn. Open DAY06/DRILL.md.
