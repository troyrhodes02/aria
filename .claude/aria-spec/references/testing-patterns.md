# Testing Patterns

Standard test patterns for A.R.I.A. feature specs.

A.R.I.A. handles financial data, optional time data, revenue entities, and insights. Testing must prove not just that the system works, but that it works **correctly, deterministically, and safely**.

---

## Test Case Format

Use `GIVEN / WHEN / THEN` structure:

```text
TEST: descriptive_snake_case_name
GIVEN:
  - Preconditions describing initial state
  - Specific entity states
  - Tenant/workspace ownership
  - Relevant configuration or prior data
WHEN:
  - API call with specific payload, OR
  - System event that triggers behavior, OR
  - Background processing operation
THEN:
  - Expected state changes
  - Expected response format
  - Expected derived values
  - Side effects
  - Invariants preserved
```

Every test should make it obvious:
- what data exists
- what action occurs
- what must be true afterward

---

## Required Test Categories

Every spec must include tests in these categories:

### 1. Happy Path Tests

Basic operations that should succeed:

```text
TEST: create_resource_basic
GIVEN:
  - Valid tenant context
  - Valid input data
WHEN:
  - POST /resources with valid payload
THEN:
  - Resource created with all required fields
  - 201 response returned
  - tenantId assigned correctly
  - Timestamps set correctly
```

Use this category for: creation, retrieval, updates, normal processing flows.

---

### 2. State Transition Tests

Valid and invalid lifecycle changes:

```text
TEST: valid_state_transition
GIVEN:
  - Resource in state A
WHEN:
  - Action transitions resource to state B
THEN:
  - State changes to B
  - Transition side effects occur
  - Derived values recalculate if required

TEST: invalid_state_transition
GIVEN:
  - Resource in state A
WHEN:
  - Action requires state B
THEN:
  - 400 response with invalid_state_transition
  - State remains unchanged
  - No side effects occur
```

Use for: sync states, mapping review states, ingestion lifecycle, archival transitions.

---

### 3. Validation Tests

Input validation and business rule enforcement:

```text
TEST: validation_required_field
GIVEN:
  - Request missing required field
WHEN:
  - POST /resources
THEN:
  - 400 response with validation_error
  - details.field identifies missing field
  - No partial data is created

TEST: validation_business_rule
GIVEN:
  - Request violates business rule
WHEN:
  - Action attempted
THEN:
  - Appropriate error returned
  - No partial changes committed
```

Use for: required fields, invalid enum values, bad ownership references, invalid merges, unsupported mappings.

---

### 4. Edge Case Tests

Boundary conditions and unusual scenarios:

```text
TEST: empty_collection
GIVEN:
  - No resources match filter
WHEN:
  - GET /resources?filter=nonexistent
THEN:
  - 200 response
  - Empty array returned
  - total = 0

TEST: pagination_boundary
GIVEN:
  - 75 resources exist
WHEN:
  - GET /resources?limit=50&offset=50
THEN:
  - 25 items returned
  - total = 75

TEST: concurrent_modification
GIVEN:
  - Two requests target same resource
WHEN:
  - Both attempt conflicting updates
THEN:
  - Exactly one succeeds or conflict behavior is deterministic
  - Data integrity remains intact
```

Use for: empty states, partial mappings, duplicate imports, stale updates, small and large datasets.

---

### 5. Data Integrity Tests

System properties that must remain true:

```text
TEST: integrity_no_orphaned_children
GIVEN:
  - Any system state
THEN:
  - Every child record has a valid parent
QUERY:
  - Verification query
EXPECT:
  - 0 invalid rows

TEST: integrity_exact_aggregation
GIVEN:
  - Known set of financial records
WHEN:
  - Aggregation runs
THEN:
  - Total equals exact expected value
  - No rounding error is introduced
```

Use for: foreign key validity, deduplication guarantees, exact totals, required ownership, mapping consistency.

---

### 6. Tenant Isolation Tests

A.R.I.A. must strictly isolate data by tenant:

```text
TEST: tenant_cannot_access_other_tenant_resource
GIVEN:
  - Tenant A resource exists
  - Authenticated user belongs to Tenant B
WHEN:
  - Tenant B requests Tenant A resource
THEN:
  - 403 or 404 returned per system convention
  - No data from Tenant A is exposed

TEST: tenant_scoped_list_only_returns_owned_data
GIVEN:
  - Tenant A and Tenant B each have resources
WHEN:
  - Tenant A requests list endpoint
THEN:
  - Only Tenant A resources are returned
```

This category is mandatory for: all resource endpoints, all list queries, all mutation endpoints, all derived views and aggregates.

---

### 7. Determinism Tests

Derived outputs must be stable and explainable:

```text
TEST: deterministic_same_input_same_output
GIVEN:
  - Fixed input dataset
WHEN:
  - Processing logic runs twice
THEN:
  - Output is identical both times

TEST: deterministic_insight_generation
GIVEN:
  - Revenue entity contributes 45 percent of total revenue
WHEN:
  - Insight generation runs
THEN:
  - Same concentration insight is generated each time
  - Priority and wording remain consistent
```

Use for: insight generation, revenue entity grouping rules, mapping logic, trend calculations.

---

### 8. Integration Scenarios

Full lifecycle tests mirroring real user workflows:

```text
TEST: lifecycle_complete_workflow
SCENARIO: Description of real-world workflow

STEP 1: Initial action
VERIFY:
  - Expected state after step 1

STEP 2: Follow-up action
VERIFY:
  - Expected state after step 2

STEP 3: Final action
VERIFY:
  - Expected output visible to user
```

Use for: onboarding flows, integration connection → sync → normalization → display, mapping confirmation → dashboard visibility, time mapping → efficiency insight generation.

---

## A.R.I.A.-Specific Test Requirements

These must be tested whenever relevant.

### Financial Correctness

If a feature touches revenue, expenses, totals, or trends, verify:
- exact amounts
- exact counts
- exact grouping behavior
- no floating point drift

### Time Data Reliability

If a feature uses calendar/time data, verify:
- timestamps normalize correctly
- duration calculations are correct
- weak or missing mappings do not create misleading outputs

### Insight Validity

If a feature generates insights, verify:
- trigger conditions are correct
- no insight is produced when conditions are not met
- explanation matches underlying data
- suggested action aligns with the actual condition

### Manual Correction Boundaries

If manual review or correction exists, verify:
- manual actions correct the current issue
- system does not repeatedly require the same manual action
- corrected data affects downstream output correctly

---

## Test Data Factories

Include TypeScript factory functions:

```typescript
function createTestResource(overrides?: Partial<Resource>): Resource {
  return {
    id: uuid(),
    tenantId: "test-tenant",
    name: "Test Resource",
    state: "ACTIVE",
    createdAt: new Date(),
    updatedAt: new Date(),
    ...overrides,
  };
}
```

**Factory guidelines:**
- Provide sensible defaults for all required fields
- Always include `tenantId` where relevant
- Accept partial overrides
- Use `uuid()` for IDs
- Use `new Date()` for timestamps
- Name clearly: `createTest{EntityName}`

---

## Derived Value Verification

For computed outputs, verify exact derivation:

```text
THEN:
  - revenueTotal = 125000
  - mappedTimeMinutes = 300
  - estimatedHourlyValue = 25000
  - result matches expected formula
```

> Do not stop at checking presence. Verify correctness.

---

## Error Response Verification

Be specific:

```text
THEN:
  - Response: 409 Conflict
  - Response body:
    - error: "duplicate_resource"
    - message: contains "already exists"
    - existing_resource includes summary fields
    - resolution_options includes supported choices
```

---

## Database State Verification

For non-trivial operations, verify persistence state:

```text
THEN:
  - Resource A updated as expected
  - Resource B archived if required
  - Join records created correctly
  - No orphaned records remain
  - No records created for wrong tenant
```

---

## Negative Tests

Always test what must not happen:

```text
TEST: archived_resource_immutable
GIVEN:
  - Resource is archived
WHEN:
  - Mutation attempted
THEN:
  - Request rejected
  - State unchanged
  - No downstream recalculation occurs
```

Examples:
- no cross-tenant access
- no invalid merge
- no insight from unreliable time data
- no mutation without permission
- no duplicate import when dedupe should catch it

---

## Performance Considerations

For operations at scale:

```text
TEST: bulk_operation_performance
GIVEN:
  - 1000 resources to process
WHEN:
  - Bulk action runs
THEN:
  - Completes within acceptable time
  - Memory remains bounded
  - No N+1 query pattern appears
```

> In v1, correctness is more important than optimization, but obvious scaling hazards should still be tested when relevant.
