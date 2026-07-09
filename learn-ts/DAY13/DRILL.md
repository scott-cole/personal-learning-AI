# DAY13: Drill — "Interactive Counter"

Build a small browser app in `DAY13/`.

## File structure

```
DAY13/
  index.html
  index.ts
```

## Requirements

### index.html
```html
<!DOCTYPE html>
<html>
<head><title>Counter App</title></head>
<body>
  <div id="app">
    <h1>Counter: <span id="count-display">0</span></h1>
    <button id="increment">+1</button>
    <button id="decrement">-1</button>
    <button id="reset">Reset</button>
    <input id="step-input" type="number" value="1" min="1" />
    <p>Step size: <span id="step-display">1</span></p>
  </div>
  <script src="index.js"></script>
</body>
</html>
```

### index.ts
```typescript
// 1. Select all needed elements with proper types
// 2. Create a Counter class with:
//    - private count: number
//    - private step: number
//    - increment(), decrement(), reset(), setStep(n: number)
//    - render() — updates the display
// 3. Wire up event listeners for each button
// 4. Handle input change for step size
```

## Expected behavior

- Clicking +1 increments by the step size
- Clicking -1 decrements by the step size
- Reset sets count back to 0
- Changing the input updates the step size and the step display
- The counter display always shows the current count

## Hints

<details>
<summary>Click for hints</summary>

- Use `!` (non-null assertion) if you're sure the element exists: `document.getElementById("x")!`
- The input's `value` is a string — convert with `Number(input.value)`
- `input` event fires on every change, including typing
- The `render` method should update `textContent` of the display spans
</details>

## How to run

You need a local server or compile with tsc:

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY13
npx tsc index.ts --outDir . --target ES2020 --module ES2020
# Then open index.html in a browser
```

Or use Vite:
```bash
npx vite .
```

## When you're done

Show me your `index.ts` file and a screenshot of the working counter.
