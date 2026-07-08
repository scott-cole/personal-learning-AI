# DAY09: Drill — "Design an API for a bookshop"

You're designing a REST API for an online bookshop. Don't write code — just design the URLs, methods, and status codes.

## The bookshop needs

1. List all books (with optional filtering by author and genre)
2. Get details of a single book
3. Add a new book to the catalogue
4. Update a book's price
5. Remove a book from the catalogue
6. List all orders for a specific customer
7. Place a new order
8. Mark an order as shipped
9. Cancel an order
10. List all reviews for a book
11. Add a review to a book
12. Get customer details

## Your task

For each of the 12 operations, specify:

| Operation | HTTP method | URL | Success status | Error statuses |
|-----------|-------------|-----|----------------|----------------|
| List books | GET | /books | 200 | — |
| ... | ... | ... | ... | ... |

## Example

| # | Method | URL | Success | Errors |
|---|--------|-----|---------|--------|
| 1 | GET | `/books?author=...&genre=...` | 200 | — |
| 2 | GET | `/books/{id}` | 200 | 404 |
| 3 | POST | `/books` | 201 | 400, 422 |

## Deliverables

Fill out the table for all 12 operations. Think about:
- Should nested resources exist? (`/books/{id}/reviews`)
- What about filtering, sorting, pagination?
- What status codes make sense for each operation?

## Hints

<details>
<summary>Click for hints</summary>

- Operations 10 and 11: nested under `/books/{id}/reviews`
- Operation 6: `/customers/{id}/orders`
- Operation 6 could also be `/orders?customerId={id}`
- Updating (operation 8) uses PATCH for partial update
- Cancelling (operation 9) could be a PATCH with status change or a DELETE depending on design
</details>
