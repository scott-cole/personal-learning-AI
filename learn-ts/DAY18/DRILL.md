# DAY18: Drill — "Utility Type Playground"

Write a TypeScript program in `DAY18/index.ts`.

## Requirements

```typescript
interface Employee {
  id: number;
  name: string;
  email: string;
  department: string;
  salary: number;
  startDate: Date;
  isActive: boolean;
}

// 1. Declare a type `NewEmployeePayload` using Omit (exclude id, startDate, isActive)
// 2. Declare a type `EmployeeUpdate` using Partial + Omit (any field except id)
// 3. Declare a type `EmployeeDirectory` using Record — keyed by department string,
//    values are Employee[] (arrays!)
// 4. Declare a type `EmployeeSummary` using Pick (only id, name, department)

// Then write runtime functions:
// 5. `createEmployee(payload: NewEmployeePayload): Employee`
//    - Generates id = Date.now(), startDate = new Date(), isActive = true
//    - Logs the created employee
// 6. `updateEmployee(id: number, changes: EmployeeUpdate): void`
//    - Logs "Updating #ID:" and the changes
// 7. `groupByDepartment(employees: Employee[]): EmployeeDirectory`
//    - Groups employees by department using reduce
```

## Example output

```
Created: { id: 1700000000, name: "Alice", email: "alice@co.com", department: "Eng", ... }

Updating #1: { salary: 120000 }

Department "Eng": 2 employees
Department "Sales": 1 employee
```

## Hints

<details>
<summary>Click for hints</summary>

- `Omit<Employee, "id" | "startDate" | "isActive">` for the payload
- `Partial<Omit<Employee, "id">>` for the update — but the id field is passed separately
- `Record<string, Employee[]>` for the directory
- For `groupByDepartment`, use `.reduce((acc, emp) => { ...; return acc; }, {} as EmployeeDirectory)`
</details>

## Bonus

Add a utility type `Writable<T>` that removes `readonly` (if present). Use it to create a mutable version of a `Readonly<Employee>`.

## How to run

```bash
cd ~/Dev/learn-ts/DAY18
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
