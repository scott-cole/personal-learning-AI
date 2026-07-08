# DAY03: Checkpoint

**Closed book.**

1. What's the difference between a function and a method?
   a function isnt related to a struct, a method is a function on a struct

2. You have `func (u User) getName() string` and `func (u *User) setName(n string) string`. Why does `setName` use a pointer but `getName` doesn't?
   get name is just reading the value from a struct, set name is changing the value on the struct, it points to the original not a copy.

3. Create a `Book` struct with `Title` and `Author` fields, then create an instance.
   type Book struct {
   Title string
   Author string
   }

   b := Book {
   Title: "Book Title",
   Author: "Scott Cole",
   }

4. What is `p.Name` if you do `var p Person` and Person has a `Name string` field?
   this would be the Name string value of the Person struct.

5. In code review you see:

   ```go
   type Config struct {
       Timeout int
   }
   func (c Config) SetTimeout(t int) { c.Timeout = t }
   ```

   What bug will the caller encounter?

   this will change a copy not the original Timeout on the struct because were not using a pointer reciever
