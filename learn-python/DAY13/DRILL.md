# DAY13 Drill — The Vehicle Family Tree

## Task

Write a Python script (`drill.py`) demonstrating inheritance with vehicles.

### Classes

1. **`Vehicle`** (base)
   - `__init__(self, make, model, year)`
   - `start()` — returns `"Engine started"`
   - `stop()` — returns `"Engine stopped"`
   - `info()` — returns `"{year} {make} {model}"`
   - `__str__` — returns the info string

2. **`Car(Vehicle)`**
   - `__init__` adds `doors` param
   - `honk()` — returns `"Beep beep!"`
   - Override `info()` to include door count

3. **`Motorcycle(Vehicle)`**
   - `__init__` adds `has_sidecar` param (default `False`)
   - `wheelie()` — returns `"Popping a wheelie!"`
   - Override `info()` to include sidecar info

4. **`Fleet`** (composition)
   - `__init__(self)` — maintains a list of vehicles
   - `add(self, vehicle)` — adds a vehicle
   - `list_vehicles(self)` — prints all vehicles with their info and a call to `start()`

### Expected output

```
=== FLEET ===
--- Vehicle 1 ---
2010 Ford Mustang (2 doors)
Engine started
Beep beep!

--- Vehicle 2 ---
2022 Harley-Davidson Iron 883 (with sidecar)
Engine started
Popping a wheelie!

--- Vehicle 3 ---
2021 Toyota Camry (4 doors)
Engine started
Beep beep!
```

### Hints

- `super().__init__(make, model, year)` in both `Car` and `Motorcycle`
- `Motorcycle.__init__` should pass `has_sidecar` up or store it directly
- `Fleet` uses composition — it "has-a" list of vehicles

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Define `Vehicle` base class
- [ ] Define `Car(Vehicle)` with override
- [ ] Define `Motorcycle(Vehicle)` with override
- [ ] Define `Fleet` using composition
- [ ] Create instances and demonstrate polymorphism
