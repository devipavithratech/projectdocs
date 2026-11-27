# 09 · API Design

## Purpose
Define **how services expose functionality** to frontends, mobile apps, partners, and other services — styles, conventions, versioning, errors, security, and documentation.

## When to use
- Creating or changing any endpoint, event, or RPC
- Integrating with a third party
- Exposing a public/partner API
- Reviewing API pull requests

## Use cases
| Scenario | Recommended style |
|---|---|
| Public / partner API, CRUD resources | REST (+ OpenAPI) |
| Frontend needs flexible, aggregated data from many sources | GraphQL |
| Internal service-to-service, low latency | gRPC |
| Live chat, notifications, collaborative editing | WebSockets |
| Server → client one-way updates (live feed, progress) | Server-Sent Events (SSE) |
| Notify external systems of events | Webhooks |
| Async workflows between services | Message/event contracts (AsyncAPI) |

---

## User guide (non-technical)
- An **API** is a menu the app offers to other software: "you can ask for orders, create a payment, etc."
- **API documentation** (Swagger / OpenAPI page) shows every available request — QA and partners use it to test.
- **Versioning** (v1, v2) lets us improve the API without breaking existing apps.
- **Rate limits** protect the system from too many requests.

---

## Developer guide

### 1. API styles compared
| Style | Pros | Drawbacks |
|---|---|---|
| **REST** | Simple, cacheable (HTTP), universal tooling | Over/under-fetching; many round trips; inconsistent conventions across teams |
| **GraphQL** | Client chooses fields, single endpoint, strong schema | Caching harder, N+1 risk, complex auth per field, query cost attacks |
| **gRPC** | Fast (HTTP/2 + Protobuf), typed contracts, streaming | Not browser-native (needs gRPC-Web), binary = harder debugging |
| **WebSockets** | Bi-directional real-time | Stateful connections, scaling & load balancing harder |
| **SSE** | Simple server push over HTTP | One-way only; connection limits in HTTP/1.1 |
| **Webhooks** | Push events to partners | Delivery retries, signature verification, receiver downtime |

### 2. REST conventions
| Action | Method | Path | Success code |
|---|---|---|---|
| List | GET | `/v1/orders?status=paid&limit=20&cursor=abc` | 200 |
| Get one | GET | `/v1/orders/{id}` | 200 |
| Create | POST | `/v1/orders` | 201 + `Location` header |
| Replace | PUT | `/v1/orders/{id}` | 200 |
| Partial update | PATCH | `/v1/orders/{id}` | 200 |
| Delete | DELETE | `/v1/orders/{id}` | 204 |
| Action | POST | `/v1/orders/{id}/cancel` | 200 / 202 |

Rules:
- Plural nouns, kebab-case paths, camelCase (or snake_case — pick one) JSON fields.
- Use **cursor pagination** for large lists; return `nextCursor`.
- Support `Idempotency-Key` header on POSTs that create money-moving or side-effect resources.
- Timestamps in **ISO-8601 UTC** (`2026-11-27T10:15:00Z`).
- Money as `{ "amount": 49900, "currency": "INR" }` (minor units).

### 3. Status codes
| Code | Meaning |
|---|---|
| 200 / 201 / 202 / 204 | OK / Created / Accepted (async) / No content |
| 400 | Validation error |
| 401 | Not authenticated |
| 403 | Authenticated but not allowed |
| 404 | Not found |
| 409 | Conflict (duplicate, version mismatch) |
| 422 | Semantically invalid |
| 429 | Rate limited (include `Retry-After`) |
| 500 | Unexpected server error |
| 502 / 503 / 504 | Upstream / unavailable / timeout |

### 4. Error format (RFC 9457 Problem Details)
```json
{
  "type": "https://api.example.com/errors/validation",
  "title": "Validation failed",
  "status": 400,
  "detail": "email must be a valid address",
  "instance": "/v1/users",
  "traceId": "4bf92f3577b34da6a3ce929d0e0e4736",
  "errors": [{ "field": "email", "message": "invalid format" }]
}
```
Never leak stack traces or internal details in production responses.

### 5. Versioning
| Approach | Example | Drawback |
|---|---|---|
| URL path | `/v1/orders` | URL changes per version (but clearest) — **recommended** |
| Header | `Accept: application/vnd.app.v2+json` | Less visible, harder to test in browser |
| Query param | `?version=2` | Easy to forget, caching issues |

Policy: additive changes (new optional fields/endpoints) are non-breaking. Removing/renaming fields or changing types is breaking → new version + deprecation period (`Deprecation` & `Sunset` headers).

### 6. Security
- AuthN: **OAuth 2.0 / OIDC** with JWT access tokens (short-lived) or API keys for server-to-server partners.
- AuthZ: check permissions on every request (object-level — prevent IDOR).
- Validate all input (schema validation: zod, Joi, Pydantic, Bean Validation).
- Rate limiting & quotas per client.
- CORS: allowlist specific origins.
- HTTPS only; HSTS.
- See [Security](10-security.md).

### 7. Documentation & contract-first
- Write **OpenAPI 3.x** spec (REST), **GraphQL SDL**, **.proto** (gRPC), **AsyncAPI** (events).
- Generate docs (Swagger UI, Redoc, Scalar), client SDKs (openapi-generator), and mocks (Prism).
- Lint specs (Spectral) in CI; contract-test with **Pact** or schema diff tools to catch breaking changes.

### 8. API gateway responsibilities
Auth, rate limiting, request routing, TLS termination, request/response logging, CORS, caching. Tools: Kong, AWS API Gateway, Azure APIM, Apigee, Traefik, NGINX, Envoy.

---

## Drawbacks & trade-offs
| Drawback | Mitigation |
|---|---|
| Breaking changes break clients | Versioning, contract tests, deprecation policy |
| Chatty APIs (many calls) | BFF (Backend-for-Frontend), GraphQL, aggregate endpoints |
| GraphQL abuse (deep queries) | Depth/complexity limits, persisted queries |
| Inconsistent conventions | Shared API style guide + Spectral linting |
| Webhook unreliability | Retries with backoff, signatures, event replay endpoint |
| Over-designing for flexibility | Design for known consumers; iterate |

## Checklist
- [ ] Style chosen (REST/GraphQL/gRPC) per consumer
- [ ] Spec written & linted (OpenAPI/SDL/proto)
- [ ] Consistent naming, pagination, errors
- [ ] AuthN/AuthZ on every endpoint
- [ ] Idempotency for side-effect POSTs
- [ ] Rate limits configured
- [ ] Versioning & deprecation policy followed
- [ ] Docs published

## Related docs
[Architecture](02-architecture.md) · [Security](10-security.md) · [Payment Integration](06-payment-integration.md)
