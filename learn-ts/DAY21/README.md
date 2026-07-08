# DAY21: Generic Repository Pattern (Project)

---

## "A repository is a library catalog"

A library has millions of books. If you had to search every shelf yourself, you'd never find anything. Instead, the library has a catalog — a system that says "here's how you find, add, remove, and update books."

The **Repository pattern** is that catalog for data. It's a generic interface that defines standard operations (CRUD) without caring whether the data lives in memory, a file, a database, or an API.

---

## The Repository interface

```typescript
interface Repository<T> {
  findAll(): Promise<T[]>;
  findById(id: string): Promise<T | null>;
  create(item: T): Promise<T>;
  update(id: string, item: Partial<T>): Promise<T | null>;
  delete(id: string): Promise<boolean>;
}
```

`T` is the entity type — `User`, `Product`, `Todo`, anything. The interface doesn't care *how* data is stored. It just says "these operations exist."

---

## In-memory implementation

```typescript
class InMemoryRepository<T extends { id: string }> implements Repository<T> {
  protected items: Map<string, T> = new Map();

  async findAll(): Promise<T[]> {
    return Array.from(this.items.values());
  }

  async findById(id: string): Promise<T | null> {
    return this.items.get(id) ?? null;
  }

  async create(item: T): Promise<T> {
    this.items.set(item.id, item);
    return item;
  }

  async update(id: string, changes: Partial<T>): Promise<T | null> {
    const existing = this.items.get(id);
    if (!existing) return null;
    const updated = { ...existing, ...changes };
    this.items.set(id, updated);
    return updated;
  }

  async delete(id: string): Promise<boolean> {
    return this.items.delete(id);
  }
}
```

---

## File-based implementation

```typescript
import { readFileSync, writeFileSync } from "fs";

class FileRepository<T extends { id: string }> implements Repository<T> {
  private items: Map<string, T> = new Map();

  constructor(private filePath: string) {
    this.load();
  }

  private load(): void {
    try {
      const data = JSON.parse(readFileSync(this.filePath, "utf-8"));
      this.items = new Map(data.map((item: T) => [item.id, item]));
    } catch {
      this.items = new Map();
    }
  }

  private save(): void {
    const data = Array.from(this.items.values());
    writeFileSync(this.filePath, JSON.stringify(data, null, 2));
  }

  async create(item: T): Promise<T> {
    this.items.set(item.id, item);
    this.save();
    return item;
  }

  // ... other methods call .save() after mutations
}
```

---

## Using the repository

```typescript
interface User {
  id: string;
  name: string;
  email: string;
}

async function main(): Promise<void> {
  const repo = new InMemoryRepository<User>();

  const alice = await repo.create({
    id: "1",
    name: "Alice",
    email: "alice@example.com",
  });

  const bob = await repo.create({
    id: "2",
    name: "Bob",
    email: "bob@example.com",
  });

  const all = await repo.findAll();
  console.log(all.length); // 2

  const found = await repo.findById("1");
  console.log(found?.name); // "Alice"

  await repo.update("1", { name: "Alice Smith" });
  const updated = await repo.findById("1");
  console.log(updated?.name); // "Alice Smith"

  await repo.delete("2");
  console.log((await repo.findAll()).length); // 1
}

main();
```

---

## Week 3 recap

| Day | Skill | Used in project |
|-----|-------|----------------|
| DAY15 | keyof, typeof | Generic constraints `T extends { id: string }` |
| DAY16 | Mapped types | Optional transformations |
| DAY17 | Conditional types | Type filtering utilities |
| DAY18 | Utility types | Partial, Pick, Omit in update/payload types |
| DAY19 | Declaration files | Types for the repository |
| DAY20 | tsconfig | Project configuration |

Your turn. Open DAY21/DRILL.md.
