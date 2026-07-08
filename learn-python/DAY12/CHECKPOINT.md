# DAY12 Checkpoint

## Questions

1. What does `self` represent in a class method?
2. What is the difference between `__str__` and `__repr__`?
3. What does the `@property` decorator do?
4. What is a class method and how is it different from a static method?
5. How do you call the parent class's `__init__` from a subclass?

<details>
<summary>Answers</summary>

1. The instance of the class that the method was called on.
2. `__str__` is for end-user readability (used by `print()`); `__repr__` is for developers (used by `repr()`), ideally reproducing the object.
3. It lets you define a method that can be accessed like an attribute (no `()` needed), with optional getter/setter/deleter logic.
4. A class method receives `cls` (the class) and can access/modify class-level state; a static method receives neither `self` nor `cls` — it's just a function namespaced in the class.
5. `super().__init__(arguments)` — usually called in the subclass's `__init__`.

</details>
