# A.R.I.A. Coding Guidelines

Project conventions and patterns for AI-assisted development.

A.R.I.A. handles financial data, optional calendar/time data, revenue entities, and generated insights. The codebase must prioritize:
- correctness
- trust
- tenant isolation
- deterministic behavior
- minimal unnecessary complexity

---

## Core Engineering Principles

### 1. Correctness Before Convenience
If a shortcut risks incorrect totals, bad mappings, misleading insights, or tenant leakage, do not take it.

### 2. Small, Clear Systems Beat Flexible Systems
Prefer a direct implementation that solves the current feature well over a generalized abstraction built for imagined future requirements.

### 3. Deterministic Outputs
The same input data should produce the same structured outputs and insights unless the underlying data changes.

### 4. Trust-Sensitive Engineering

Because A.R.I.A. works with financial and calendar data:
- totals must be exact
- access boundaries must be strict
- logs must avoid sensitive payload leakage
- "read-only" expectations must be preserved in implementation

---

## Database Operations

### Transaction Safety

**Always wrap multi-step database operations in transactions when they must succeed or fail together.**

Use Prisma transactions for:
- multi-step entity updates
- merge/split operations
- normalization pipelines that write multiple dependent records
- mapping confirmation flows that update multiple related models
- any workflow where partial success would leave the system inconsistent

```typescript
// Pattern: Auto-transaction when no external tx provided
export async function updateEntity(
  id: string,
  input: UpdateInput,
  tx?: PrismaClient,
): Promise<Entity> {
  if (!tx) {
    return prisma.$transaction(async (txClient) => {
      return updateEntityInternal(id, input, txClient as PrismaClient);
    });
  }

  return updateEntityInternal(id, input, tx);
}

async function updateEntityInternal(
  id: string,
  input: UpdateInput,
  client: PrismaClient,
): Promise<Entity> {
  await client.entity.update({ ... });
  const updated = await client.entity.create({ ... });
  return updated;
}
```

---

### Tenant Scoping

Every persisted business record must be scoped to a tenant when applicable.

**Requirements:**
- All queries must include `tenantId` filters where appropriate
- Resource ownership must be checked before reads or mutations
- Never trust IDs alone without tenant validation
- Cross-tenant access must be impossible by default

```typescript
const entity = await prisma.revenueEntity.findFirst({
  where: {
    id: entityId,
    tenantId,
  },
});
```

---

### Financial Data Storage

Financial values must be stored in the smallest unit possible:
- cents, not dollars
- integers, not floats

Do not use floating point arithmetic for financial calculations.

---

### Time Data Storage

Time-related records should:
- store timestamps in UTC
- preserve source timestamps when needed for debugging
- normalize durations into consistent units
- avoid silent timezone ambiguity

---

## Service Layer Patterns

### Deterministic Business Logic

Service functions that classify, map, aggregate, or generate derived outputs must be deterministic.

**Avoid:**
- hidden randomness
- unstable ordering
- ambiguous matching rules
- behavior that depends on incidental DB ordering

**Be explicit about:**
- thresholds
- tie-breaking
- fallback behavior
- missing-data handling

---

### Mapping and Classification Rules

For entity detection, classification, and matching:
- prefer explicit, traceable rules
- keep matching simple in MVP
- do not introduce fuzzy or AI-like logic unless explicitly specified
- preserve reviewability when confidence is not strong

---

### Derived Data Boundaries

If a value is computed, be clear about:
- where it is computed
- when it is recomputed
- what source records it depends on

Do not bury core business logic in the UI layer.

---

## Client-Side Data Fetching

### SWR for Reads and Mutations

Use SWR for client-side server communication. Avoid raw fetch logic directly inside components.

- **Reads:** use `useSWR` via hooks in `src/hooks/`
- **Mutations:** use `useSWRMutation` from `swr/mutation`

```typescript
import useSWRMutation from "swr/mutation";

async function createEntity(url: string, { arg }: { arg: CreateEntityBody }) {
  const res = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(arg),
  });

  if (!res.ok) {
    const err = await res.json();
    throw new Error(err.message || "Something went wrong");
  }

  return res.json();
}

const { trigger, isMutating, error, reset } = useSWRMutation(
  "/api/v1/entities",
  createEntity,
);

await trigger(body);
```

**Why:**
- consistent loading and error handling
- easier revalidation
- reduced boilerplate
- fewer ad hoc state patterns

---

### UI Rules for A.R.I.A.

UI components should:
- display structured, already-prepared data
- avoid reimplementing backend business logic client-side
- clearly distinguish exact values from review-required or inferred values
- avoid misleading precision

If data may be incomplete or uncertain, the UI must reflect that honestly.

---

## API Routes

### Error Handling

Use centralized error handlers that map domain errors to HTTP responses.

```typescript
function handleServiceError(error: unknown): NextResponse {
  if (error instanceof EntityNotFoundError) {
    return errorResponse(404, error.code, error.message);
  }

  if (error instanceof ZodError) {
    return errorResponse(400, "validation_error", formatZodError(error));
  }

  throw error;
}
```

---

### API Requirements

All API routes must:
- authenticate the user
- enforce tenant scoping
- validate inputs
- return predictable error shapes
- avoid leaking sensitive internals

---

### Sensitive Logging Rules

**Do not log:**
- access tokens
- raw financial payloads
- raw calendar payloads
- personally identifying metadata unless masked

**Prefer logging:**
- internal IDs
- tenant IDs
- counts
- state transitions
- sanitized summaries

---

## Testing

### Test-Driven Development (TDD)

All new features follow TDD:

1. **Red** — write failing tests
2. **Green** — implement the minimum code to pass
3. **Refactor** — improve while keeping tests green

When implementing a new feature:
1. Create the test file first
2. Write tests for happy paths, edge cases, and error cases
3. Add minimal file scaffolding if needed
4. Run tests to verify they fail for the right reason
5. Implement until all relevant tests pass

---

### Required Test Priorities for A.R.I.A.

Every meaningful feature should test for relevant subsets of:
- happy path behavior
- validation
- edge cases
- state transitions
- data integrity
- tenant isolation
- deterministic outputs
- integration scenarios

---

### Financial Correctness Tests

If a feature touches revenue, expenses, totals, trends, or derived value calculations, tests must verify **exact numeric outcomes**.

---

### Insight Validity Tests

If a feature generates insights:
- verify the trigger condition
- verify the insight is not generated when it should not be
- verify explanation matches the source data
- verify outputs are stable across repeated runs

---

### Factory Pattern

Use factories in `src/test/factories.ts` for creating test data.

```typescript
const entity = await createRevenueEntity(prisma, {
  tenantId: tenant.id,
  name: "Acme Corp",
});
```

---

### Test Isolation

Tests should clean up or isolate data to prevent pollution.

```typescript
beforeEach(async () => {
  await prisma.$transaction([
    prisma.insight.deleteMany(),
    prisma.revenueEntity.deleteMany(),
    prisma.transaction.deleteMany(),
    prisma.tenant.deleteMany(),
  ]);
});
```

---

## Frontend Implementation

### Design Fidelity

When implementing from a design doc or preview:
- treat the design as the target
- match hierarchy, spacing, and interaction behavior closely
- include loading, empty, and error states where specified

---

### Product Tone in UI

A.R.I.A. UI should feel:
- clear
- operational
- high-trust
- calm
- not overly decorative

---

### Insight Visibility

If a feature's value depends on surfacing an insight or key business understanding:
- make that information visually obvious
- do not bury it behind secondary interaction
- do not let supporting UI overpower the core answer

---

## Workflow

When finishing work on a Linear issue:

1. Commit all changes with a descriptive message referencing the Linear issue ID
2. Push the branch and create a PR using `gh pr create`
3. Include in the PR:
   - Linear ticket link
   - summary of what changed
   - key models/files added or modified
   - migration or schema changes
   - test status
   - base branch name
4. Move the ticket to the appropriate review state if the workflow supports it

---

## Scope Discipline

When implementing, do not:
- expand the ticket because "it will help later"
- add infrastructure not required for the current feature
- introduce additional dashboards, analytics, or controls outside the accepted scope
- build predictive or AI-driven behavior unless explicitly defined
- turn a lightweight correction flow into a management system

If a needed improvement is real but out of scope: note it clearly and leave it for a future ticket.

---

## Done Means

A feature is only done when:

- [ ] acceptance criteria are satisfied
- [ ] implementation matches the spec
- [ ] data correctness is preserved
- [ ] tenant boundaries are enforced
- [ ] tests pass
- [ ] lint/typecheck/required repo checks pass
- [ ] no obvious trust or scope violations were introduced
