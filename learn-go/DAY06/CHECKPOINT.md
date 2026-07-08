# DAY06: Checkpoint

**Closed book.**

1. What does `&` do? What does `*` do (when used before a type vs before a variable)?

- is a pointer to a type, & is a reference to a variable

2. What does `greeting` become after this code?

   ```go
   func upper(s string) { s = strings.ToUpper(s) }
   greeting := "hello"
   upper(greeting)
   ```

   " " not changing the original only changing a copy

3. How would you fix the code above so that `greeting` becomes "HELLO"?
   func upper(s *string) { *s = strings.ToUpper(\*s) }
   greeting := "hello"
   upper(&greeting)

4. What does this print? Why?

   ```go
   var p *int
   fmt.Println(p == nil)
   ```

   nil, changing the original int

5. In code review you see:
   ```go
   type Config struct { Timeout int }
   func loadConfig() Config { ... }
   ```
   The struct is 50 fields. Would you recommend changing this to return `*Config`? Why or why not?

yes becasue \*Config is handling memory and not portantially GB of data.
