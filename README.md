# Simple Book API — Postman QA Collection

This repository contains a Postman collection for QA-style testing of a “Simple Book API”. It’s designed as a lightweight regression suite that validates API availability, book catalog behavior, and the complete order lifecycle (create → read → update → delete) using chained requests and variables.

Collection: [Simple_Book_API](collection/49001497-b8f5d00d-3faf-487f-a5e8-3bcbfa0d8dd5)

---

## What this collection tests

### 1) Health/readiness
- **API Status** — checks API availability  
  Request: [API Status](request/49001497-84571872-a201-460b-939f-9525c6e8189c)

### 2) Book catalog coverage
- **List books** (supports filtering like `type=non-fiction`)  
  Request: [List of books](request/49001497-ce00f28c-d859-4fdc-9d85-bb2f14d2a465)
- **Get a single book by ID**  
  Request: [Get single book](request/49001497-25720664-f071-4cb8-ae1b-b5cf088b5959)

### 3) Client + ordering workflows
- **Register API client** (used to enable authenticated/token flows where required)  
  Request: [Register API Client](request/49001497-35cdb8f2-fce5-4b97-8666-abdc1f561540)
- **Create an order**  
  Request: [Order book](request/49001497-7727ea26-9814-4c6d-b3b9-49e38f9d53cd)
- **List orders**  
  Request: [Get all book orders](request/49001497-64ab47cd-8386-4959-bdf5-e05cacd25c7e)
- **Get / Update / Delete an order by `orderId`**  
  Requests:
  - [Get a book order](request/49001497-535f6834-2760-43f7-9a12-9ac5a43c8b57)
  - [Update an order](request/49001497-acc59041-c86f-4073-bdbb-ac36998c879f)
  - [Delete an order](request/49001497-e1b20dde-42e1-470a-8c93-45b201bddea9)

---

## Variables & configuration

### Environment
This collection expects an environment with:

- `baseUrl` — the API base URL (example: `https://<host>`)

Active environment in this workspace: `Simple_API_Environment.`

### Globals used for chaining
These IDs are typically set/used during runs:

- `bookId` — a selected book identifier used by “Get single book” and ordering flows
- `orderId` — the created order identifier used by get/update/delete order requests

### Token variable (if applicable)
The collection also references an `accessToken` variable (used for authenticated order flows where required by the API).

---

## Recommended execution order (QA regression flow)

1. [API Status](request/49001497-84571872-a201-460b-939f-9525c6e8189c)
2. [List of books](request/49001497-ce00f28c-d859-4fdc-9d85-bb2f14d2a465) (captures/uses `bookId` for downstream requests)
3. [Get single book](request/49001497-25720664-f071-4cb8-ae1b-b5cf088b5959)
4. [Register API Client](request/49001497-35cdb8f2-fce5-4b97-8666-abdc1f561540) (if token/client is required)
5. [Order book](request/49001497-7727ea26-9814-4c6d-b3b9-49e38f9d53cd) (creates `orderId`)
6. [Get all book orders](request/49001497-64ab47cd-8386-4959-bdf5-e05cacd25c7e)
7. [Get a book order](request/49001497-535f6834-2760-43f7-9a12-9ac5a43c8b57)
8. [Update an order](request/49001497-acc59041-c86f-4073-bdbb-ac36998c879f)
9. [Delete an order](request/49001497-e1b20dde-42e1-470a-8c93-45b201bddea9)

---

## How to run (Postman)

1. Import or open the collection in Postman.
2. Select your environment and ensure `baseUrl` is set.
3. Run the collection using the Collection Runner.
4. Review assertions/tests per request and the overall run results to confirm regressions.

---

## QA notes / goals

- **Regression-friendly:** designed to be re-runnable with minimal manual edits.
- **Variable-driven:** uses `baseUrl` plus IDs (`bookId`, `orderId`) passed between requests.
- **Covers CRUD:** validates the full lifecycle of an order, including cleanup via delete.
