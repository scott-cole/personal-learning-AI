# DAY05: Checkpoint

**Closed book.**

1. What happens if you read a key that doesn't exist in a map?
   you will get teh zero value

2. How do you safely check if a key exists in a map without mistaking a zero value for "not found"?
   comma, ok

3. What happens when you do this?

   ```go
   var m map[string]int
   m["hello"] = 1
   ```

   you would get a nil map error PANIC

4. What's the difference between a map and a slice? When would you use one over the other?
   a slice is ordered and a map isnt, a slice is indexed by position and map indexed by key

should use maps for lookups.

5. You have `users := map[string]int{"alice": 30, "bob": 25}`. Write the Go code to loop through it and print each key-value pair.

for i, v := range users {
fmt.Println(i, v)
}
