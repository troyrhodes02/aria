---
name: aria-spec
description: Generate detailed technical specifications for A.R.I.A. features. Use when creating new feature specs, updating existing specs, or when the user mentions "spec", "specification", "technical design", or needs to document a feature for implementation. Specs define data models, system behavior, APIs, and testing to support A.R.I.A.’s clarity-first, insight-driven product. Requires a design document as input for UI/UX context.
---

# A.R.I.A. Specification Generator

Generate detailed technical specifications for A.R.I.A. features following consistent, implementation-ready conventions.

A.R.I.A. is a system that transforms financial and time data into clear, actionable insights with minimal user input. The purpose of a spec is not just to define how something is built, but to ensure the system produces correct, trustworthy, and meaningful outputs.

Specs must prioritize:
- correctness of financial data
- clarity of derived outputs
- minimal user intervention
- reliable automation
- strong tenant isolation and data integrity

## Where Specs Fit

```text
Pitch Document → Design Document → Technical Spec
(why + scope)    (what users see)   (how it's built)

Pitch defines problem, value, scope, and monetization

Design Doc defines what the user sees and how they interact

Spec defines how the system produces that experience reliably and correctly

Prerequisites

Before writing a spec, ensure you have:

Design document — Required. Defines UI structure, flows, and user-facing behavior

Existing data models — Review current Prisma schema and system entities

A.R.I.A. system context — Understand how this feature fits into:

tenants/workspaces

financial data ingestion

revenue entities

time data (optional)

insight generation

Related artifacts when applicable

feature breakdowns

existing specs

roadmap context

Spec File Conventions

Filename: feature-name-spec.md (kebab-case)

Location: Project docs directory or as specified by user

Format: Markdown with Prisma code blocks for data models

Core Technical Principles for A.R.I.A.

Every spec must enforce:

1. Data Accuracy Is Non-Negotiable

Revenue, expenses, and totals must be correct

No lossy transformations without explicit reasoning

No assumptions that could mislead the user

2. Tenant Isolation Is Mandatory

All data must be scoped to a tenant/workspace

No cross-tenant access paths

Queries must always include tenant constraints

3. Automation Over Manual Input

Systems should infer, classify, and group where possible

Manual correction is allowed, but should not be required repeatedly

4. Insight Readiness

Data structures should support insight generation

Avoid raw storage that cannot produce meaningful outputs

5. Simplicity Over Flexibility

Avoid building generalized systems “for later”

Model only what is required for the current feature

Required Sections

Every spec must include these sections in order:

1. Metadata Header
---
version: 1.0.0
status: draft | review | approved
author: [name]
last_updated: YYYY-MM-DD
design_doc_reference: [link or filename to source design doc]
---
2. Summary

2–3 paragraphs covering:

What the feature does (one sentence)

The core system behavior or abstraction introduced

What new capability the system gains

Success criteria (when is this feature considered working)

Focus on outcomes, not implementation details.

3. Problem

Current system limitations

What the system cannot represent or compute today

What questions cannot be answered

Why this blocks clarity or insight generation

This should connect directly to the pitch and design doc.

4. Core Concepts

Define each entity or concept introduced or modified:

Field	Description
field_name	What it represents

Include:

relationships between concepts

ownership (tenant, system, derived)

required vs optional fields

distinctions between similar concepts

Concepts must be defined in a way that supports:

correct aggregation

insight generation

future extension without overgeneralization

5. States & Lifecycle

Define system states and transitions.

Include:

valid states (enum)

allowed transitions

triggers for each transition

invalid transitions and how they are handled

Examples:

ingestion states

mapping states

sync states

validation states

6. UI Integration

Reference the design doc for layout and UX. This section defines how backend logic supports it.

Screens

For each screen:

Purpose

Data required

Key elements (table)

Actions supported

Visual indicators and derived values

Modals & Flows

Trigger conditions

Required data

Validation rules

Side effects (data updates, recalculations)

Derived Data Requirements

Explicitly define:

what values are computed

where they are computed (API vs DB vs client)

when they update

7. Data Model
Relationship to Existing Schema
From	Relation	To	Description
New Entities

Use Prisma schema format:

model EntityName {
  id        String   @id @default(uuid())
  tenantId  String

  // fields...

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([tenantId])
}

enum EnumName {
  VALUE_ONE
  VALUE_TWO
}
Required Updates to Existing Models
// Add to ExistingModel
model ExistingModel {
  // existing fields...

  // NEW: relation
  newRelation  NewEntity[]
}
Data Integrity Rules

Define constraints such as:

uniqueness

required relationships

deduplication rules

consistency guarantees

8. Data Processing & Logic

Define how raw data becomes structured data and insights.

Include:

Ingestion Logic (if applicable)

source of data

transformation steps

normalization rules

Classification Logic

how transactions are categorized (revenue vs expense)

mapping to entities

Mapping Logic

how relationships are formed (e.g., transaction → entity)

confidence or validation rules (if applicable)

Aggregation Logic

how totals, trends, and metrics are computed

grouping rules

Update Behavior

when recalculation occurs

what triggers updates

All logic must be deterministic and explainable.

9. API Surface
Base URL
/api/v1
Authentication

Bearer token required

Must enforce tenant scoping on all endpoints

Endpoints

For each endpoint:

#### [Action] [Resource]

[METHOD] /path/:param

**Query Parameters:**
| Param | Type | Description |

**Request:**
```json
{
  "field": "value"
}

Response:

{
  "field": "value"
}

Error Responses:


---

### Error Response Format

```json
{
  "error": "error_code",
  "message": "Human readable description",
  "details": {}
}

Standard error codes:

Code	HTTP Status	Description
not_found	404	Resource does not exist
validation_error	400	Invalid request payload
unauthorized	401	Missing or invalid auth
forbidden	403	Authenticated but not permitted
10. Testing Strategy

Use GIVEN / WHEN / THEN format.

TEST: test_name_snake_case
GIVEN:
  - Preconditions
WHEN:
  - Action
THEN:
  - Expected outcome
Required Test Categories

Happy Path

Validation

Edge Cases

State Transitions

Data Integrity

Tenant Isolation

Integration Scenarios

Test Data Factories
function createTestEntity(overrides?: Partial<Entity>): Entity {
  return {
    id: uuid(),
    tenantId: "test-tenant",
    ...overrides,
  };
}
11. Acceptance Criteria

Grouped checklist:

1. **[Feature Area]**
   - [ ] Specific testable behavior
   - [ ] Data correctness verified
   - [ ] UI receives correct data
12. Explicit Non-Goals (v1)

Use ❌ prefix:

❌ Feature not included

❌ Future extension deferred

❌ Additional complexity not required

Include when applicable:

Manual correction is acceptable in v1

No predictive or AI-based behavior

No additional dashboards beyond scope

13. Open Questions

Numbered list of unresolved decisions:

Topic — description of tradeoff or missing detail

14. Future Considerations

How this feature enables future features

What may expand later

What systems could evolve from this

Style Guidelines
Code Blocks

Prisma for schema

TypeScript for logic/tests

JSON for API examples

Detail Level

Be explicit about types, nullability, and constraints

Define how values are computed

Include both success and failure cases

Avoid vague system behavior

Tone

Direct and technical

Use “must”, “should”, “may” intentionally

No ambiguous requirements

Workflow

Receive design doc

Understand system impact

Identify required data and behavior

Clarify unknowns when necessary

Define core concepts

Design data model

Define logic and transformations

Define API surface

Define testing strategy

Finalize and validate against design doc

Interview Guidelines

When clarifying with the user:

Focus on missing logic, not UI

Ask about:

data relationships

edge cases

update timing

correctness guarantees

Keep questions minimal and high-impact

Use existing project context first

Output Quality Standard

A strong A.R.I.A. spec makes it obvious:

how data flows through the system

how correctness is guaranteed

how insights become possible

how tenant data is protected

how the UI receives accurate, structured data

If the spec defines structure but not behavior, it is incomplete.