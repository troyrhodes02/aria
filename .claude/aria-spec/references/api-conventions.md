# API Conventions

Standard patterns for A.R.I.A. API endpoints.

A.R.I.A. APIs are designed to:
- safely handle financial and time data
- enforce strict tenant isolation
- provide deterministic, reliable outputs
- support insight generation without ambiguity

---

## Base Configuration

```text
Base URL: /api/v1
Authentication: Bearer token in Authorization header
Content-Type: application/json

All requests must be authenticated and scoped to a tenant.
```

---

## Tenant Scoping *(CRITICAL)*

Every request must be scoped to a tenant (workspace).

**Requirements**
- All queries must filter by `tenantId`
- No cross-tenant data access is allowed
- Resource IDs must be validated against tenant ownership
- Unauthorized access must return `403 forbidden`

**Example**

```typescript
// Always enforce tenant scoping
where: {
  id: resourceId,
  tenantId: currentTenantId
}
```

> Failure to enforce tenant scoping is a critical violation.

---

## Endpoint Naming

Use plural nouns for collections:
- `/transactions`
- `/revenue-entities`
- `/insights`

Use nested resources when ownership is explicit:
- `/revenue-entities/:id/transactions`

Use verbs for non-CRUD actions:
- `/transactions/:id/reclassify`
- `/revenue-entities/:id/merge`

Prefer clarity over brevity in naming.

---

## Request Patterns

### List *(GET collection)*

```
GET /resources?limit=50&offset=0&sort=created_at&order=desc
```

**Standard query params:**

| Param    | Type   | Default      | Description                  |
| -------- | ------ | ------------ | ---------------------------- |
| `limit`  | int    | 50           | Max results (max: 200)       |
| `offset` | int    | 0            | Pagination offset            |
| `sort`   | string | `created_at` | Sort field                   |
| `order`  | string | `desc`       | Sort order: `asc` or `desc`  |

**Filtering**
- Exact match: `?field=value`
- Partial match: `?field_search=partial`
- Multiple values: `?field=value1,value2`
- Date range: `?created_after=ISO8601` / `?created_before=ISO8601`

**Response Envelope**

```json
{
  "resources": [...],
  "total": 142,
  "limit": 50,
  "offset": 0
}
```

---

### Get *(GET single)*

```
GET /resources/:id
```

- Returns full resource object
- Must validate tenant ownership
- Returns `404` if not found or not accessible

---

### Create *(POST)*

```
POST /resources
```

- Request body contains resource fields
- Server assigns `id`, timestamps, and `tenantId`
- Returns created resource

---

### Update *(PATCH)*

```
PATCH /resources/:id
```

- Partial updates only
- Must validate tenant ownership
- Returns updated resource

---

### Soft Delete *(Preferred)*

```
POST /resources/:id/archive
```

- Avoid hard deletes
- Mark resource as inactive/archived
- Preserve historical data for insights

---

### Actions

```
POST /resources/:id/action-name
```

Used for:
- transformations
- mapping changes
- classification updates
- merge/split operations

Response depends on action.

---

## Response Patterns

### Success *(2xx)*

```json
{
  "id": "uuid",
  "field": "value",
  "created_at": "2025-01-15T09:14:00Z",
  "updated_at": "2025-01-15T09:22:00Z"
}
```

- Timestamps must use ISO8601 format
- Always include timezone

### Created *(201)*
- Return created resource
- Include `Location` header

### No Content *(204)*

Used for:
- actions with no response body
- archive operations (optional)

### Error *(4xx, 5xx)*

```json
{
  "error": "error_code",
  "message": "Human readable description",
  "details": {
    "field": "specific field error"
  }
}
```

**Standard Error Codes**

| Code                      | HTTP Status | When to Use                          |
| ------------------------- | ----------- | ------------------------------------ |
| `not_found`               | 404         | Resource doesn't exist               |
| `validation_error`        | 400         | Invalid request payload              |
| `invalid_state_transition`| 400         | Action not allowed in current state  |
| `duplicate_resource`      | 409         | Unique constraint violation          |
| `conflict`                | 409         | State or data conflict               |
| `unauthorized`            | 401         | Missing or invalid auth token        |
| `forbidden`               | 403         | Authenticated but not permitted      |
| `rate_limited`            | 429         | Too many requests                    |
| `internal_error`          | 500         | Unexpected server error              |

---

## Conflict Resolution Pattern

Used when:
- duplicate entities are detected
- mappings conflict
- merge/split decisions are required

**Response**

```json
{
  "error": "duplicate_resource",
  "message": "Resource already exists",
  "existing_resource": {
    "id": "uuid",
    "...": "summary fields"
  },
  "resolution_options": ["merge", "replace", "keep_both"]
}
```

**Resolution Endpoint**

```
POST /resources/:id/resolve-conflict
```

```json
{
  "resolution": "merge",
  "target_id": "uuid"
}
```

---

## Pagination

Use offset-based pagination for v1:

```
GET /resources?limit=50&offset=100
```

Response must include: `total`, `limit`, `offset`

---

## Sorting

Single field:
```
?sort=field&order=asc
```

Default: `order=desc` (newest first)

---

## Idempotency

For POST operations that may be retried:
- Accept `Idempotency-Key` header
- Store request hash + response
- Return cached response for duplicate key within 24h

---

## Data Sensitivity & Logging Rules

A.R.I.A. handles financial and time data. Logging must be controlled.

**Do NOT log:**
- full financial transaction payloads
- customer-identifiable metadata (emails, names) unless masked
- access tokens or secrets

**Allowed:**
- internal IDs
- aggregated values
- masked identifiers

---

## Integration Safety Rules

For external integrations (Stripe, Plaid, PayPal, Google):
- Treat all external data as untrusted input
- Validate and normalize before storage
- Never assume schema stability
- Enforce read-only behavior where applicable
- Verify webhook authenticity (signature validation)

---

## Determinism Requirement

APIs that return computed values must be:
- **deterministic** — same input → same output
- **explainable** — derivation can be traced
- **consistent** across requests

No randomization or hidden variability is allowed.

---

## Time & Financial Data Standards

- All timestamps stored in UTC
- Currency values must be stored in smallest unit (e.g., cents)
- Avoid floating point arithmetic for financial calculations
- Aggregations must be exact

---

## Performance Guidelines *(v1)*

- Optimize for correctness over speed
- Avoid premature optimization
- Ensure queries are indexed on: `tenantId` and commonly filtered fields

---

## Future Considerations

- Cursor-based pagination for large datasets
- Event-driven updates for real-time insights
- Rate limiting by tenant
