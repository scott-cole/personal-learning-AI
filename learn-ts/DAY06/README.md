# DAY06: Classes, Access Modifiers (public, private, protected)

---

## "A class is a cookie cutter"

You have a cookie cutter shaped like a star. Press it into dough and you get a star cookie. Press it again — another star cookie. Same shape, different cookies.

A class is a cookie cutter for objects. You define the shape once, then create (instantiate) as many objects as you need:

```typescript
class Cookie {
  shape: string = "star";
  flavor: string;

  constructor(flavor: string) {
    this.flavor = flavor;
  }

  eat(): void {
    console.log(`Eating a ${this.flavor} ${this.shape} cookie. Yum!`);
  }
}

const first = new Cookie("chocolate");
const second = new Cookie("vanilla");
first.eat();  // "Eating a chocolate star cookie. Yum!"
second.eat(); // "Eating a vanilla star cookie. Yum!"
```

`new Cookie(...)` presses the cutter. Each instance gets its own `flavor` but shares the same methods.

---

## constructor — the oven settings

The `constructor` runs when you create a new instance. It's where you set up initial values:

```typescript
class User {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

const user = new User("Alice", 30);
```

TypeScript has a shorthand to skip the boilerplate:

```typescript
class User {
  constructor(
    public name: string,
    public age: number,
  ) {}
}
```

The `public` keyword in the constructor parameter automatically creates and assigns the property. This is the idiomatic TypeScript way.

---

## Access modifiers — who can touch what?

Cookies on a counter are public — anyone can grab one. The recipe book in the drawer is private — only the baker can see it. The family recipe is protected — family members can use it, but strangers can't.

| Modifier | Accessible where |
|----------|-----------------|
| `public` | Anywhere (default) |
| `private` | Only inside the class |
| `protected` | Inside the class and subclasses |

```typescript
class BankAccount {
  public owner: string;
  private balance: number;
  protected accountNumber: string;

  constructor(owner: string, initialBalance: number) {
    this.owner = owner;
    this.balance = initialBalance;
    this.accountNumber = Math.random().toString(36).slice(2);
  }

  public deposit(amount: number): void {
    this.balance += amount;
  }

  public getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount("Alice", 1000);
console.log(account.owner);     // ✅ "Alice"
console.log(account.balance);   // ❌ Property 'balance' is private
console.log(account.accountNumber); // ❌ Property 'accountNumber' is protected
account.deposit(500);
console.log(account.getBalance()); // 1500
```

---

## Inheritance — cookie cutters from cookie cutters

A class can extend another class, inheriting its properties and methods:

```typescript
class Animal {
  constructor(public name: string) {}

  speak(): void {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name: string, public breed: string) {
    super(name); // must call parent constructor first
  }

  speak(): void {
    console.log(`${this.name} barks!`);
  }
}

const dog = new Dog("Rex", "Husky");
dog.speak(); // "Rex barks!"
```

`super()` calls the parent constructor. Always call it before accessing `this` in a child class.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Class declaration | `class Foo { }` |
| Constructor shorthand | `constructor(public name: string)` |
| Private field | `private balance: number` |
| Protected field | `protected id: string` |
| Inheritance | `class Dog extends Animal` |
| Call parent constructor | `super(name)` |
| Override method | Define same method name in child class |

Your turn. Open DAY06/DRILL.md.
