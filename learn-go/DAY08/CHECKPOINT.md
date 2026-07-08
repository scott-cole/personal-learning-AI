# DAY08: Checkpoint

**Closed book.**

1. What does it mean that Go interfaces are "implicitly satisfied"?
   aslong as it meets the interface fields it will be treated as part of that interface and dont have to says satisfies interface

2. Write an interface called `Reader` with a `Read(p []byte) (int, error)` method.

type Reader interface {
read Read(p []byte) (int, error)
}

3. Why does `fmt.Println` work with any type? (Hint: what interface does it accept?)

i dont know, i assume the Format interface?

4. What's the #1 Go idiom about accepting and returning types?

accept interfaces return structs

5. You have a function `func process(s Storage)` and two types `memoryStore` and `sqlStore` that both satisfy `Storage`. What makes this design powerful?

you can use both types without changing the process as it satisfies Storage
