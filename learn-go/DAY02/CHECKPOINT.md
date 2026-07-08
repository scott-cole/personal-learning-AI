# DAY02: Checkpoint

**Closed book.**

1. What two things does `fmt.Println` return? (Hint: nothing discussed, but think about it)
   this will return a int and error

2. In this function signature, what does the first `int` represent? What does the second `int` represent?

   ```go
   func add(x int, y int) int
   ```

   the first int is the first number of this addition, y is the 2nd number

3. Write the Go code to call a function `getUser(id string) (string, error)` and handle the error.
   user, err := getUser(id)
   if err != nil {
   fmt.Println("Error:", err)
   return
   }
   return user, nil

4. What does `nil` mean in the context of errors?
   nil means null, no errors

5. You see this in code review:

   ```go
   result, _ := someFunction()
   ```

   What question would you ask the author?

   why are we ignoring the error here?

---

**Check your answers against the lesson.**
