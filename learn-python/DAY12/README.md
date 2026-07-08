# DAY12 — Classes & OOP Basics

## Anecdote: A class is a cookie cutter

In a bakery, the cookie cutter sits on the shelf — a stainless-steel outline of a star. By itself, it produces nothing. But when you press it into rolled dough, you get a star-shaped cookie. Press it again — another star, identical in shape, but a distinct cookie. You can ice one with pink frosting and another with blue. Each cookie is an *instance*, born from the same *class*.

`class CookieCutter:` is the metal shape. `cookie1 = CookieCutter()` is one pressed cookie. The `__init__` method is the moment the cutter hits the dough — it sets up each cookie's initial properties (shape, size). Methods like `frost("pink")` are things the cookie can do after it exists.

---

## Defining a class

```python
class Dog:
    """A simple dog class."""

    species = "Canis familiaris"  # class attribute — shared by all instances

    def __init__(self, name, age):
        """Instance initializer — called when you create a new Dog."""
        self.name = name  # instance attribute
        self.age = age

    def bark(self):
        """Instance method."""
        return f"{self.name} says Woof!"

    def __str__(self):
        """Called by print() and str()."""
        return f"{self.name} ({self.age} years old)"

    def __repr__(self):
        """Called by repr() — should look like a valid Python expression."""
        return f"Dog('{self.name}', {self.age})"
```

```python
# Creating instances
buddy = Dog("Buddy", 3)
max = Dog("Max", 5)

print(buddy.name)          # Buddy
print(buddy.bark())        # Buddy says Woof!
print(buddy.species)       # Canis familiaris
print(buddy)               # Buddy (3 years old)
print(repr(buddy))         # Dog('Buddy', 3)
```

---

## `self`

`self` is the instance itself. Python passes it automatically when you call a method on an instance.

```python
buddy.bark()       # Python calls Dog.bark(buddy)
```

The name `self` is a convention — you could call it `this`, but you shouldn't.

---

## `__init__` — the constructor

Sets up initial state. Always returns `None`.

```python
class Point:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

p = Point(3, 4)
print(p.x, p.y)   # 3 4
```

---

## `__str__` vs `__repr__`

| Method | For | Goal |
|--------|-----|------|
| `__str__` | End-user readability | `print(obj)` — informal |
| `__repr__` | Developer debugging | `repr(obj)` — unambiguous, ideally recreates the object |

If you define only `__repr__`, it's used for `__str__` as well.

---

## `@property` — computed attributes

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value

    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2

    @property
    def diameter(self):
        return self._radius * 2

c = Circle(5)
print(c.radius)      # 5 (getter)
print(c.area)        # 78.539...
c.radius = 10        # (setter)
# c.radius = -1      # ValueError
```

Properties let you start with plain attributes and later add logic without changing the API.

---

## Class methods and static methods

```python
class Employee:
    company = "TechCorp"

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    @classmethod
    def change_company(cls, new_name):
        cls.company = new_name

    @classmethod
    def from_string(cls, data):
        """Alternative constructor: parse 'name,salary'."""
        name, salary = data.split(",")
        return cls(name, int(salary))

    @staticmethod
    def is_workday(day):
        """Check if a given day is a workday (Mon-Fri)."""
        return day.weekday() < 5

emp = Employee.from_string("Alice,75000")
print(emp.name, emp.salary)    # Alice 75000
```

| Decorator | First param | Use |
|-----------|-------------|-----|
| (none) | `self` | Instance methods |
| `@classmethod` | `cls` | Factory methods, class-level state |
| `@staticmethod` | nothing | Utility functions related to the class |
