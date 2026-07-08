# DAY03: Drill — "Model a User"

You're building a user system. You need to model users with a struct and attach behaviour with methods.

## Requirements

Write a `main.go` in `DAY03/`:

```go
package main

import "fmt"

// 1. Define a User struct with:
//    - Name (string)
//    - Email (string)
//    - Age (int)
//    - IsActive (bool)
type User struct {
    // TODO
}

// 2. Method: sayHello — returns "Hi, I'm [Name]"
func (u User) sayHello() string {
    // TODO: return "Hi, I'm " + u.Name
}

// 3. Method: celebrateBirthday — increments Age by 1
//    Must use a pointer receiver (modify the original)
func (u *User) celebrateBirthday() {
    // TODO: u.Age++
}

// 4. Method: deactivate — sets IsActive to false
func (u *User) deactivate() {
    // TODO
}

func main() {
    // Create a user
    u := User{
        Name: "Scott",
        Email: "scott@example.com",
        Age: 30,
        IsActive: true,
    }

    // Test sayHello
    fmt.Println(u.sayHello())

    // Test celebrateBirthday
    u.celebrateBirthday()
    fmt.Printf("Age after birthday: %d\n", u.Age)

    // Test deactivate
    u.deactivate()
    fmt.Printf("Is active: %v\n", u.IsActive)
}
```

## Expected output

```
Hi, I'm Scott
Age after birthday: 31
Is active: false
```

## Why this matters

In production, you'll have structs like `Order`, `Transaction`, `Payment`. Each has methods that operate on the data. This is how domain logic is organised in Go.

## Run it

```bash
cd ~/Dev/learn-go/DAY03
go run main.go
```
