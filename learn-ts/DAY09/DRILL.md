# DAY09: Drill — "Payment Processor"

Write a TypeScript program in `DAY09/index.ts`.

## Requirements

```typescript
enum PaymentStatus {
  Pending = "pending",
  Completed = "completed",
  Failed = "failed",
  Refunded = "refunded",
}

// Discriminated union for payment methods
type PaymentMethod =
  | { kind: "credit"; cardNumber: string; expires: string }
  | { kind: "paypal"; email: string }
  | { kind: "crypto"; walletAddress: string; currency: "BTC" | "ETH" };

interface Payment {
  id: string;
  amount: number;
  status: PaymentStatus;
  method: PaymentMethod;
}
```

Then:

```typescript
// 1. Write a function processPayment(payment: Payment): string
//    - Use a switch on payment.status
//    - Return a message like "Payment #abc is pending"
//    - For completed payments, include the method info
//    - For failed payments, include " — please retry"
//    - For refunded, include " — refund issued"

// 2. Write a function formatPaymentMethod(method: PaymentMethod): string
//    - Use a switch on method.kind
//    - For credit: "Credit card ending in XXXX"
//    - For paypal: "PayPal (email@example.com)"
//    - For crypto: "Crypto (BTC wallet: 0x...)"

// 3. Create at least 4 sample payments with different statuses/methods
//    Process and print each one
```

## Example output

```
Payment #1: $50.00 via Credit card ending in 1234 — pending
Payment #2: $25.00 via PayPal (alice@example.com) — completed
Payment #3: $100.00 via Crypto (BTC wallet: 0xABC) — failed, please retry
Payment #4: $75.00 via Credit card ending in 5678 — refunded, refund issued
```

## Hints

<details>
<summary>Click for hints</summary>

- Use `switch` on `payment.status` and `payment.method.kind` separately
- `crypto` property `currency` is a literal union `"BTC" | "ETH"` — use it in the message
- Generate ids with `Math.random().toString(36).slice(2, 6)`
- `.toFixed(2)` for dollar amounts
</details>

## Bonus

Add a function `totalByStatus(payments: Payment[], status: PaymentStatus): number` that sums amounts for a given status.

## How to run

```bash
cd ~/Dev/learn-ts/DAY09
npx tsx index.ts
```

## When you're done

Show me your `index.ts` file.
