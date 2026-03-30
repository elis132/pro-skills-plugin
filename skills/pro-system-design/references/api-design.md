# API Design: Practical Principles

## The Foundation

An API is a contract. Every decision you make now is something you (or someone else) will live with for years. Be intentional.

## REST Design

### URL Structure

Resources are nouns. HTTP methods are verbs. Don't repeat yourself.

```
# Good
GET    /orders          → List orders
POST   /orders          → Create order
GET    /orders/123      → Get order 123
PATCH  /orders/123      → Update order 123
DELETE /orders/123      → Delete order 123

# Bad
GET    /getOrders
POST   /createOrder
POST   /orders/123/update
GET    /orders/delete/123
```

Nested resources for clear ownership:
```
GET    /orders/123/items       → Items in order 123
POST   /orders/123/items       → Add item to order 123
```

But don't nest more than two levels deep. Beyond that, promote the resource:
```
# Too deep
GET /users/1/orders/2/items/3/reviews

# Better
GET /reviews?item_id=3
```

### HTTP Status Codes

Use the right ones. Not just 200 and 500.

**Success:**
- `200 OK` for successful GET, PATCH, DELETE
- `201 Created` for successful POST that creates something (include Location header)
- `204 No Content` for successful DELETE with no response body
- `202 Accepted` for async operations ("we got it, processing later")

**Client Errors:**
- `400 Bad Request` for malformed input (validation errors)
- `401 Unauthorized` for missing/invalid authentication
- `403 Forbidden` for valid auth but insufficient permissions
- `404 Not Found` for resource doesn't exist
- `409 Conflict` for state conflicts (duplicate, optimistic locking failure)
- `422 Unprocessable Entity` for valid syntax but semantic errors
- `429 Too Many Requests` for rate limiting (include Retry-After header)

**Server Errors:**
- `500 Internal Server Error` for unexpected failures (log these, alert on these)
- `503 Service Unavailable` for planned downtime or overload

### Error Response Format

Be consistent. Always return the same error structure:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Order total must be positive",
    "details": [
      {
        "field": "total",
        "message": "Must be greater than 0",
        "value": -50
      }
    ]
  }
}
```

Machine-readable `code` for programmatic handling. Human-readable `message` for debugging. `details` for field-level validation errors.

Never expose internal error messages, stack traces, or SQL queries in production error responses.

### Pagination

For any endpoint that returns a list, paginate from day one. You will regret not doing this.

**Cursor-based pagination** (preferred):
```json
{
  "data": [...],
  "next_cursor": "abc123",
  "has_more": true
}
```
Request: `GET /orders?cursor=abc123&limit=20`

Cursor pagination is stable when data is being inserted/deleted. Offset pagination breaks (skip duplicates, show items twice).

**Offset-based** (simpler, fine for small datasets):
```json
{
  "data": [...],
  "total": 342,
  "offset": 40,
  "limit": 20
}
```

### Filtering and Sorting

Keep it simple. Query parameters for filters:
```
GET /orders?status=pending&created_after=2025-01-01&sort=-created_at&limit=20
```

Prefix with `-` for descending sort. Don't invent a query language unless you're building a developer platform.

### Timestamps

Always UTC. Always ISO 8601. No exceptions.
```json
{
  "created_at": "2025-06-15T14:30:00Z",
  "updated_at": "2025-06-15T14:35:22Z"
}
```

### IDs

Use UUIDs for public-facing IDs if you don't want to leak information about your data (sequential IDs reveal how many records exist and when they were created). Use sequential integers internally for joins and indexes.

Alternative: prefixed IDs like `ord_abc123`, `usr_def456`. These are self-documenting and prevent "wrong ID type" bugs.

## Authentication

### For most projects: Token-based

```
Authorization: Bearer <token>
```

Use JWTs if you need the token to carry claims (user ID, roles) without a database lookup. Use opaque tokens with a database lookup if you need instant revocation. JWTs can't be revoked before expiry without a blocklist (which defeats the purpose).

**Short-lived access tokens** (15 min to 1 hour) + **long-lived refresh tokens** (days to weeks) is the standard pattern.

### For machine-to-machine: API Keys

Simple, effective, understood by everyone. Send as a header:
```
X-API-Key: sk_live_abc123
```

Prefix keys with environment: `sk_live_` for production, `sk_test_` for testing. This prevents the "I accidentally used my production key in development" problem.

### Rate Limiting

Implement from day one for any public endpoint. Return `429` with `Retry-After` header.

Simple approach: sliding window per API key, stored in your database or application memory. You don't need Redis for this unless you have multiple application instances.

## Versioning

**Don't version until you need to.** Premature versioning adds complexity. Version when you need to make a breaking change and can't migrate all consumers.

When you do version, URL prefix is simplest:
```
/v1/orders
/v2/orders
```

Header-based versioning is "cleaner" but harder for consumers to use. URL versioning is obvious and debuggable.

## Common Mistakes

### Chatty APIs
An API that requires 5 round trips to render a page is a bad API. Design endpoints around USE CASES, not database tables. If the frontend always needs order + items + customer together, make an endpoint that returns all three.

### Inconsistent naming
Pick a convention and stick to it. `snake_case` for JSON field names is the most common in REST APIs. Don't mix `camelCase` and `snake_case`.

### No idempotency
`POST /orders` should be safe to retry. Use an idempotency key:
```
Idempotency-Key: unique-request-id-123
```
If the same key is sent twice, return the original response instead of creating a duplicate.

### Ignoring partial updates
Use `PATCH` with partial payloads, not `PUT` with full replacement. `PUT` requires the client to send the entire resource, which is error-prone and wasteful.

### Not documenting
At minimum: OpenAPI/Swagger spec generated from your code. If you have external consumers, human-readable docs with examples. Every endpoint should have at least one request/response example.
