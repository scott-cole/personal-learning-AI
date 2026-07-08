# DAY13 Checkpoint

## Questions

1. How do you call the parent class's `__init__` from a child class?
2. What is polymorphism?
3. What is the difference between inheritance and composition?
4. What does `ClassName.__mro__` show?
5. What happens if you try to instantiate a class that inherits from `ABC` and has unimplemented `@abstractmethod`s?

<details>
<summary>Answers</summary>

1. `super().__init__(arguments)` in the child's `__init__`.
2. The ability of different classes to respond to the same method name with their own implementation.
3. Inheritance is "is-a" (a Dog is an Animal); composition is "has-a" (a Car has an Engine).
4. The Method Resolution Order — the order Python searches for methods in the class hierarchy.
5. It raises `TypeError: Can't instantiate abstract class ...` with a message listing the unimplemented methods.

</details>
