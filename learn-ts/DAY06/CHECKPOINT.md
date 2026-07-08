# DAY06: Checkpoint

**Closed book.** No looking back.

1. What's the difference between `private` and `protected`?

2. What does the shorthand `constructor(public name: string) {}` do?

3. Why must you call `super()` in a child class constructor?

4. If a parent class has a `private` field `id`, can a child class access it?

5. What happens if you don't use `new` before `ClassName()`?

---

<details>
<summary>Answers</summary>

1. `private` is accessible only within the class. `protected` is accessible within the class and its subclasses.
2. It automatically declares `this.name` as a public property and assigns the constructor parameter to it.
3. To call the parent class constructor and initialize inherited properties before `this` is available.
4. No. `private` members are only accessible within the class that defines them, not by subclasses.
5. You'll get a runtime error — `this` will be `undefined` and properties won't be set correctly.
</details>
