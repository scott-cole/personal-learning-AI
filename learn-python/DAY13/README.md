# DAY13 — Inheritance & Polymorphism

## Anecdote: Inheritance is a family tree

Every kitchen has a base recipe for "dough": flour, water, yeast. From that one recipe, you can derive "pizza dough" (add olive oil, salt) and "sweet dough" (add sugar, butter). The base gives its children a head start — they inherit all the ingredients and steps, then add or override what makes them unique.

Python inheritance works the same. A **parent class** (base recipe) defines shared attributes and methods. **Child classes** inherit everything, then specialise. If a child doesn't like a method, it **overrides** it with its own version. This is **polymorphism** — calling the same method name on different classes and getting appropriate behaviour.

---

## Basic inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return "..."
    
    def __str__(self):
        return self.name

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class Duck(Animal):
    def speak(self):
        return "Quack!"

animals = [Dog("Buddy"), Cat("Whiskers"), Duck("Donald")]
for a in animals:
    print(f"{a} says {a.speak()}")
# Buddy says Woof!
# Whiskers says Meow!
# Donald says Quack!
```

---

## `super()` — call the parent

Use `super()` to call the parent class's methods.

```python
class Vehicle:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year

    def info(self):
        return f"{self.year} {self.make} {self.model}"

class Car(Vehicle):
    def __init__(self, make, model, year, doors):
        super().__init__(make, model, year)  # call parent __init__
        self.doors = doors

    def info(self):
        return f"{super().info()} — {self.doors} doors"

car = Car("Toyota", "Corolla", 2020, 4)
print(car.info())   # 2020 Toyota Corolla — 4 doors
```

---

## Multiple inheritance

A class can inherit from multiple parents.

```python
class Flyer:
    def fly(self):
        return "Flying..."

class Swimmer:
    def swim(self):
        return "Swimming..."

class Duck(Flyer, Swimmer):
    def quack(self):
        return "Quack!"

d = Duck()
print(d.fly())     # Flying...
print(d.swim())    # Swimming...
```

### Method Resolution Order (MRO)

Python uses the C3 linearisation algorithm. You can inspect it:

```python
print(Duck.__mro__)
# (<class '__main__.Duck'>, <class '__main__.Flyer'>, <class '__main__.Swimmer'>, <class 'object'>)
```

---

## Composition vs inheritance

**Inheritance**: "A Dog IS-AN Animal"
**Composition**: "A Car HAS-AN Engine"

```python
# Composition — preferred over inheritance in many cases
class Engine:
    def start(self):
        return "Engine running"

class Wheels:
    def rotate(self):
        return "Wheels spinning"

class Car:
    def __init__(self):
        self.engine = Engine()    # HAS-A Engine
        self.wheels = [Wheels() for _ in range(4)]  # HAS-4 Wheels

    def drive(self):
        return f"{self.engine.start()} and {self.wheels[0].rotate()}"
```

| Approach | When to use |
|----------|-------------|
| Inheritance | "Is-a" relationship; shared behaviour + state |
| Composition | "Has-a" relationship; loosely coupled parts |

---

## Abstract base classes

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w = w
        self.h = h

    def area(self):
        return self.w * self.h

    def perimeter(self):
        return 2 * (self.w + self.h)

# s = Shape()  # TypeError: Can't instantiate abstract class
r = Rectangle(3, 4)
print(r.area())   # 12
```

Abstract classes cannot be instantiated — they only define the interface that children must implement.

---

## isinstance() and issubclass()

```python
print(isinstance(car, Car))       # True
print(isinstance(car, Vehicle))   # True (inheritance)
print(issubclass(Car, Vehicle))   # True
print(issubclass(Car, Animal))    # False
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Inheritance | Child class gets all parent attributes/methods |
| `super()` | Call the parent's version of a method |
| Override | Redefine a method in the child |
| Polymorphism | Same interface, different implementations |
| Multiple inheritance | A class can inherit from multiple parents |
| MRO | Method Resolution Order — `Class.__mro__` |
| Composition | "Has-a" — embed other classes as attributes |
| ABC | Abstract Base Class — define interfaces |
