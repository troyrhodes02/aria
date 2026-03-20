---
name: aria-security-review
description: Perform a structured security and data integrity review of A.R.I.A. features. Use after generating specs, APIs, or implementation plans to identify risks related to financial data handling, tenant isolation, authentication, authorization, and data leakage. Focus on preventing breaches, misuse, and silent trust failures.
---

# A.R.I.A. Security Review

Audit features for security, data integrity, and trust risks before implementation or deployment.

A.R.I.A. handles:
- financial data
- account integrations
- optional calendar/time data
- derived business insights

This means failures are not just technical — they are **trust failures**.

This skill exists to ensure:
- no data leaks
- no cross-tenant access
- no incorrect or misleading outputs
- no unsafe handling of sensitive data

---

## Core Principle

**A.R.I.A. must be safer than the user's expectations.**

Not just:
- “secure enough”
- “probably fine”
- “industry standard”

But:
- predictable
- isolated
- explainable
- non-destructive

---

## Purpose

Use this skill to review:

- technical specs
- API design
- database models
- feature logic
- integration flows
- onboarding flows
- insight systems

The goal is to identify:
- vulnerabilities
- trust-breaking edge cases
- incorrect assumptions
- silent failure risks

---

## Output Structure

---

## 1. Feature Overview

Summarize:
- what the feature does
- what data it touches
- what systems are involved

Keep concise but clear.

---

## 2. Data Sensitivity Classification

Classify all data used:

| Data Type | Sensitivity | Notes |
|----------|------------|------|
| Financial transactions | High | Exact amounts, dates |
| Revenue entities | Medium | Derived but sensitive |
| Calendar events | Medium/High | Depends on detail |
| User metadata | Low/Medium | Depends on scope |

Sensitivity levels:
- Low
- Medium
- High

---

## 3. Authentication Risks

Evaluate:

- how the user is authenticated
- whether endpoints assume auth incorrectly
- whether any route can be accessed without proper validation

Check for:
- missing auth checks
- reliance on client-side validation
- unsafe defaults

---

## 4. Authorization & Tenant Isolation

This is the most critical section.

Verify:

- all queries are tenant-scoped
- resource access is validated against tenantId
- no ID-only lookups without tenant filtering
- joins do not accidentally expose cross-tenant data

Example risks:
- fetching by ID without tenant constraint
- returning related records from another tenant
- caching data across tenants

---

## 5. Data Exposure Risks

Check for:

- API responses returning unnecessary fields
- sensitive data included in logs
- internal IDs exposed unnecessarily
- full objects returned where summaries are enough

Ensure:
- minimal required data is returned
- sensitive fields are omitted or masked where appropriate

---

## 6. Financial Data Integrity Risks

Verify:

- all financial calculations are exact
- no floating point usage
- no rounding inconsistencies
- aggregation logic is deterministic

Check:
- partial updates
- inconsistent totals
- mismatched groupings

---

## 7. Time Data Risks

If calendar/time data is used:

- ensure timestamps are normalized
- avoid timezone ambiguity
- prevent incorrect duration calculations
- ensure weak mappings do not produce misleading insights

---

## 8. Integration Risks

For external integrations:

- ensure read-only behavior is enforced
- confirm no write operations occur unless explicitly intended
- validate token handling
- ensure tokens are not logged or exposed

Check:
- token storage safety
- refresh logic
- error handling

---

## 9. Insight Trust Risks

Evaluate:

- whether insights are based on reliable data
- whether thresholds are clearly defined
- whether outputs could mislead users

Check:
- false positives
- false certainty
- insights appearing with insufficient data

---

## 10. Input Validation Risks

Verify:

- all inputs are validated (e.g., Zod or equivalent)
- invalid states are rejected
- edge cases are handled explicitly

Check:
- missing required fields
- invalid enum values
- malformed payloads

---

## 11. Error Handling Risks

Ensure:

- errors do not leak internal details
- consistent error format is used
- sensitive data is not included in error messages

---

## 12. Logging Risks

Verify logs do not include:

- raw financial data
- access tokens
- calendar payloads
- personally identifiable information unless necessary

Logs should include:
- IDs
- counts
- event types
- safe summaries

---

## 13. Concurrency & Race Conditions

Check:

- concurrent updates to the same entity
- duplicate processing
- conflicting writes

Ensure:
- transactions are used where needed
- operations are idempotent when appropriate

---

## 14. Failure Mode Analysis

Define what happens when:

- integration fails
- partial data is available
- processing is interrupted
- network issues occur

Ensure:
- system does not enter inconsistent state
- user is not shown misleading output

---

## 15. Abuse & Misuse Scenarios

Consider:

- user attempting to access other tenants' data
- repeated requests (rate abuse)
- malformed or malicious payloads
- attempting to bypass mapping or validation

---

## 16. Mitigation Recommendations

For each identified risk:

- describe the fix
- describe where it should be applied
- classify severity:
  - Critical
  - High
  - Medium
  - Low

---

## 17. Acceptance Criteria

- [ ] All endpoints enforce authentication
- [ ] All data access is tenant-scoped
- [ ] No sensitive data is leaked via API or logs
- [ ] Financial calculations are exact and deterministic
- [ ] Insight generation is trustworthy
- [ ] Input validation is enforced
- [ ] Errors are safe and consistent
- [ ] External integrations are handled securely

---

## 18. Explicit Non-Goals

- ❌ Full enterprise security architecture
- ❌ Compliance certification (SOC2, etc.)
- ❌ Advanced intrusion detection systems

Focus on:
- practical product-level safety
- correctness
- trust

---

## A.R.I.A.-Specific Rules

### 1. Tenant Isolation Is Mandatory
No exceptions. Every query must respect tenant boundaries.

### 2. Financial Data Must Be Exact
Incorrect totals = loss of trust.

### 3. Insights Must Be Honest
Do not show insights when data is insufficient.

### 4. Read-Only Means Read-Only
Integrations must not modify external systems.

### 5. Minimal Exposure
Return only what the UI needs.

---

## Workflow

1. Review feature or spec
2. Identify all data flows
3. Classify sensitivity
4. Evaluate auth + tenant isolation
5. Check data exposure
6. Validate calculations and logic
7. Analyze failure modes
8. Identify abuse scenarios
9. Recommend mitigations

---

## Quality Standard

A strong security review should answer:

- Could this leak data?
- Could this return incorrect financial information?
- Could this break tenant isolation?
- Could this mislead the user?

If the answer is “possibly,” the issue must be addressed before implementation.