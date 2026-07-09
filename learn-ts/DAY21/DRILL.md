# DAY21: Drill — "Generic Repository Implementation"

Build a generic repository system in `DAY21/`.

## File structure

```
DAY21/
  index.ts            — demo
  repository.ts       — Repository interface + InMemoryRepository
  file-repository.ts  — FileRepository (bonus)
  types.ts            — entity types
  package.json        — for running
```

## Requirements

### types.ts
```typescript
export interface Entity {
  id: string;
}

export interface User extends Entity {
  name: string;
  email: string;
  age: number;
}

export interface Product extends Entity {
  name: string;
  price: number;
  inStock: boolean;
}
```

### repository.ts
```typescript
export interface Repository<T extends Entity> {
  findAll(): Promise<T[]>;
  findById(id: string): Promise<T | null>;
  create(item: T): Promise<T>;
  update(id: string, changes: Partial<T>): Promise<T | null>;
  delete(id: string): Promise<boolean>;
}

export class InMemoryRepository<T extends Entity> implements Repository<T> {
  // Implementation from the README
}
```

### index.ts
```typescript
// 1. Create an InMemoryRepository<User>
// 2. Create 3 users
// 3. Find all users → print them
// 4. Find one user by id → print
// 5. Update one user's name → print updated
// 6. Delete one user → verify
// 7. Repeat with a Product repository
```

## Example output

```
=== Users ===
Created: [{"id":"1","name":"Alice","email":"a@b.com","age":30}]
Created: [{"id":"2","name":"Bob","email":"b@c.com","age":25}]
Created: [{"id":"3","name":"Charlie","email":"c@d.com","age":35}]

All users: 3 users found
  User 1: Alice (a@b.com)
  User 2: Bob (b@c.com)
  User 3: Charlie (c@d.com)

Found by id "2": Bob (b@c.com)

Updated user 1: Alice Smith (a@b.com)
After delete: 2 users remaining

=== Products ===
Created: Laptop ($999)
Created: Mouse ($29)
All products: 2 products
```

## Hints

<details>
<summary>Click for hints</summary>

- `Entity` interface enforces the `id: string` constraint
- `InMemoryRepository<T extends Entity>` ensures only entities with `id` can be stored
- `Partial<T>` in `update` allows updating only specific fields
- Use `crypto.randomUUID()` or `Date.now().toString()` for generating IDs
- Make the repository methods `async` even though they're synchronous — prepares for real DB later
</details>

## Bonus

Implement `FileRepository` that saves/loads from a JSON file using `fs.readFileSync`/`writeFileSync`. Add error handling for missing files.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY21
npx tsx index.ts
```

## When you're done

Show me all your files.
