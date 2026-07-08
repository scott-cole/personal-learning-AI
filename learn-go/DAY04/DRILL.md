# DAY04: Drill — "The Todo List"

You're building a simple todo list manager using slices.

## Requirements

Write a `main.go` in `DAY04/`:

```go
package main

import "fmt"

type Todo struct {
    Title     string
    Completed bool
}

func main() {
    // 1. Create an empty slice of Todo items
    // 2. Add 3 todos to your list
    //    - "Learn Go" (not completed)
    //    - "Build a URL shortener" (not completed)
    //    - "Get a backend job" (not completed)
    // 3. Print all todos in this format:
    //    [ ] Learn Go
    //    [ ] Build a URL shortener
    //    [ ] Get a backend job
    // 4. Mark "Learn Go" as completed (find it by title, set Completed = true)
    // 5. Print the list again — "[X]" for completed items
}
```

## Helper hint

To mark an item as completed, you need to find it by title. Loop through the slice with `range`, and when you find the match, `break`:

```go
for i, todo := range todos {
    if todo.Title == "Learn Go" {
        todos[i].Completed = true
        break
    }
}
```

**Why `todos[i]` and not `todo`?** Because `todo` is a COPY. Modifying it doesn't change the slice. You must modify the original at index `i`.

## Expected output

```
Todo list:
[ ] Learn Go
[ ] Build a URL shortener
[ ] Get a backend job

After completing "Learn Go":
[X] Learn Go
[ ] Build a URL shortener
[ ] Get a backend job
```

## Bonus

Write a function `addTodo(list []Todo, title string) []Todo` that adds a new todo and returns the updated slice. Call it instead of manually appending.

## Run it

```bash
cd ~/Dev/learn-go/DAY04
go run main.go
```
