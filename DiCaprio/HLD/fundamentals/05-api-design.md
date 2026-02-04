# API Design

> Principles for designing scalable, maintainable APIs.

---

## REST Basics

- **Resource-based URLs:** `/users/123`, `/orders`
- **HTTP methods:** GET (read), POST (create), PUT (replace), PATCH (partial update), DELETE
- **Stateless:** No server-side session; each request self-contained
- **Status codes:** 200, 201, 400, 401, 404, 500

---

## Key Principles

| Principle | Description |
|-----------|-------------|
| **Idempotency** | Same request N times = same result as 1 time. Critical for POST/PUT retries. |
| **Versioning** | `/v1/users` or header `Accept: application/vnd.api+json;version=1` |
| **Pagination** | Cursor-based (stable under inserts) or offset (simpler, skip expensive) |
| **Rate limiting** | Headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining` |

---

## Idempotency Keys

```
POST /payments
Idempotency-Key: uuid-from-client

→ First request: process, return 201
→ Duplicate (same key): return cached 201, don't reprocess
```

---

## Common Interview Q&A

**Q: REST vs GraphQL?**  
A: REST: simple, cacheable, over-fetch/under-fetch. GraphQL: client specifies fields, single endpoint, flexible. Use REST for simple CRUD; GraphQL for complex UIs.

**Q: How to handle breaking changes?**  
A: Versioning (URL or header). Deprecation timeline. Backward compatibility where possible.
