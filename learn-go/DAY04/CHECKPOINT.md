# DAY04: Checkpoint

**Closed book.**

1. What's the difference between `var items []string` and `items := []string{}`?
   var is nil the other is empty

2. What does `append` return?
   append returns a new slice

3. You have `names := []string{"Alice", "Bob", "Charlie"}`. Write the code to loop through it and print each name.

for \_, name := range names {
fmt.Println(name)
}

4. Why would you use `make([]string, 0, 100)` instead of `var items []string`?
   this is when we know the size of the slice we want

5. This code has a bug. Find it:
   ```go
   todos := []Todo{{Title: "Learn Go"}}
   for _, t := range todos {
       t.Completed = true
   }
   fmt.Println(todos[0].Completed) // still falsej
   ```
   t.Completed is a copy, it should be todos[i].Completed
