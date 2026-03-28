# api-design.md generation guide

## Template

```markdown
# API Design Principles

## Response Format

### Success

```json
{
  "success": true,
  "data": { ... },
  "meta": { "timestamp": "2026-01-01T00:00:00Z", "requestId": "uuid" }
}
```

### List (paginated)

```json
{
  "success": true,
  "data": [ ... ],
  "meta": { "page": 1, "size": 20, "totalCount": 150, "totalPages": 8 }
}
```

### Error

```json
{
  "success": false,
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "Order not found",
    "details": [ ... ]
  },
  "meta": { "timestamp": "2026-01-01T00:00:00Z", "requestId": "uuid" }
}
```

## HTTP Status Codes

| Code | When | Example |
|------|------|---------|
| 200 | Success (read, update) | GET /orders, PUT /orders/{id} |
| 201 | Created | POST /orders |
| 204 | Success, no body | DELETE /orders/{id} |
| 400 | Bad request | Validation failure |
| 401 | Unauthenticated | Missing/expired token |
| 403 | Forbidden | Accessing another user's resource |
| 404 | Not found | Non-existent order |
| 409 | Conflict | Duplicate creation |
| 422 | Unprocessable | Business rule violation |
| 500 | Internal error | Unexpected exception |

## URL Design

- Plural nouns: `/api/v1/orders` (not `/order`)
- Hierarchy: `/api/v1/orders/{orderId}/items`
- Actions via HTTP methods, not verbs: `POST /orders` (not `/create-order`)
- Filtering via query params: `/orders?status=pending&from=2026-01-01`
- Versioning: URL prefix (`/api/v1/`)

## DTO Naming

| Purpose | Pattern | Example |
|---------|---------|---------|
| Create request | Create{Entity}Request | CreateOrderRequest |
| Update request | Update{Entity}Request | UpdateOrderRequest |
| Single response | {Entity}Response | OrderResponse |
| List response | {Entity}ListResponse | OrderListResponse |

## Validation

Validate at the request DTO level (before controller logic).
On failure, return 400 with field-level errors:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      { "field": "email", "message": "Invalid email format" },
      { "field": "quantity", "message": "Must be ≥ 1" }
    ]
  }
}
```

## Error Code System

Domain prefix + specific situation:
- AUTH_TOKEN_EXPIRED
- ORDER_NOT_FOUND
- ORDER_ALREADY_CANCELLED
- PAYMENT_INSUFFICIENT_BALANCE
- VALIDATION_ERROR

## Authentication

- Method: {JWT / OAuth2 / Session}
- Token delivery: `Authorization: Bearer {token}`
- Authorization: {role-based / resource-owner-based}
```

## Principles

1. **Unify the response format.** When every API uses the same wrapper, the frontend can build shared handling logic.

2. **Define error codes upfront.** HTTP status codes alone aren't enough. Business error codes let clients respond appropriately.

3. **Make examples realistic.** The agent should be able to implement an API directly from this document.

4. **Adapt implementation details to the stack.** Spring Boot: @RestControllerAdvice + GlobalExceptionHandler. FastAPI: exception_handler + HTTPException. Next.js: middleware pattern.
