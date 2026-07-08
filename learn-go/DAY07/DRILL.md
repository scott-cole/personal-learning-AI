# DAY07: Week 1 Review — "CLI Todo Manager"

This is your first **mini-project**. You'll build a command-line todo list manager that runs in the terminal. It uses everything from Week 1: variables, functions, structs, slices, maps, pointers, and error handling.

## The spec

When you run the program, it shows a prompt and accepts commands:

```
Todo Manager
Commands: add <title>, list, complete <title>, delete <title>, exit

> add Learn Go
Added: Learn Go

> add Build URL shortener
Added: Build URL shortener

> list
[ ] Learn Go
[ ] Build URL shortener

> complete Learn Go

> list
[X] Learn Go
[ ] Build URL shortener

> delete Build URL shortener

> list
[X] Learn Go

> exit
Goodbye!
```

## Requirements

```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
)

type Todo struct {
    Title     string
    Completed bool
}

// You'll need these functions:
func addTodo(todos []*Todo, title string) []*Todo
func completeTodo(todos []*Todo, title string) error
func deleteTodo(todos []*Todo, title string) ([]*Todo, error)
func listTodos(todos []*Todo)
```

## Hints

- Use `bufio.NewReader(os.Stdin)` to read user input
- Use `strings.TrimSpace` to clean up input
- Use `strings.Fields` to split the command
- Store todos as `[]*Todo` (so `completeTodo` can modify the original)

## Starter code for reading input

```go
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Print("> ")
    input, _ := reader.ReadString('\n')
    input = strings.TrimSpace(input)
    
    parts := strings.Fields(input)
    if len(parts) == 0 {
        continue
    }
    
    command := parts[0]
    // ... handle command
}
```

## Complete this project

This should take you 30-60 minutes. When you're done, show me the code and I'll do a full code review.

## Run it

```bash
cd ~/Dev/learn-go/DAY07
go run main.go
```
