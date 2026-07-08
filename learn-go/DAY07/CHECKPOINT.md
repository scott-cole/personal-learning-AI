# DAY07: Week 1 Checkpoint

**This is a cumulative test of Week 1.** Answer without looking back.

1. What does `:=` do in Go?
   this creates a local variable

2. Write a function `divide(a, b int) (int, error)` that returns an error if b is 0.
   func divide(a, b int) (int, error) {
   if ( b == 0) {
   return 0, errors.New("cannot divide by 0")
   }
   return a/b, nil
   }

3. What's the output?

   ```go
   type Point struct { X, Y int }
   p := Point{X: 1, Y: 2}
   q := p
   q.X = 99
   fmt.Println(p.X)
   ```

   1

4. What's the difference between `var s []string` and `s := make([]string, 0)`?
   var is no underlying array, make is an empty array with length 0
   var is a nil slice make is an empty slice that we know the size of

5. Write the code to safely look up "admin" in a `map[string]bool` and print whether they exist.
   m := map[string]bool{"admin": true}
   if \_, exists := m["admin"]; exists {
   fmt.Println("admin exists")
   } else {
   fmt.Println("admin not found")
   }

6. What does this print?

   ```go
   func setVal(v *int) { *v = 100 }
   x := 5
   setVal(&x)
   fmt.Println(x)
   ```

   will print 100 changing the original

7. You have a slice of structs. Why does `for _, item := range items { item.Field = "new" }` not work?

item is a copy of each element. Modifying it does nothing to the original. Fix: use index:

for i := range items {
items[i].Field = "new"
}

8. What's wrong with this code?

   ```go
   var cache map[string]string
   cache["key"] = "value"
   ```

   var cache map[string]string creates a nil map. Writing to a nil map causes a runtime panic. Fix: use make:
   cache := make(map[string]string)
   cache["key"] = "value"

9. Name two reasons to use a pointer receiver instead of a value receiver.
   a pointer reciever will allow you to change the original, this is more performant and also will update the original rather than a copy

10. What does the `_` (underscore) mean in Go?
    this will ignore
