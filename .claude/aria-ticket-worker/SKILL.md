---
name: aria-ticket-worker
description: >
  End-to-end engineering agent that takes an A.R.I.A. Linear ticket from
  assignment through PR creation. Reads the ticket, finds the referenced spec,
  follows CLAUDE.md conventions, plans the work, implements the feature,
  verifies behavior, then prepares the branch and PR for review.

  Use this skill whenever the user says things like "work on ARIA-123",
  "pick up this ticket", "implement this Linear issue", "finish this ticket",
  "start on [ticket ID]", or provides a Linear issue and wants it built.
---

# Linear Ticket Worker

Takes an A.R.I.A. ticket end-to-end: ticket → spec → plan → implementation → verification → PR.

A.R.I.A. is a clarity-first product built on financial data, revenue entities, optional time data, and insight generation. Implementation work must preserve:
- correctness
- trust
- tenant isolation
- automation-first behavior
- strict scope discipline

This worker exists to execute a well-scoped ticket cleanly, not to redesign the product mid-build.

## Prerequisites

- **CLAUDE.md** at repo root
- **Spec directory** with technical specs
- **Design docs or UI previews** for frontend work when available
- **Linear CLI or MCP**
- **GitHub CLI (`gh`)**
- Any repo-specific tooling required by CLAUDE.md

## Phase 1: Gather Context

1. **Fetch the ticket**
   - Extract the ticket ID from the user input
   - Pull:
     - title
     - description
     - acceptance criteria
     - linked issues
     - parent / blocked-by relationships
     - milestone context if available

2. **Find the spec**
   - Check the ticket description for a referenced spec path
   - If not found, fuzzy-match against spec filenames
   - If still ambiguous, ask the user rather than guessing

3. **Read CLAUDE.md and the spec**
   - CLAUDE.md defines how to work in this repo
   - The spec defines what must be built
   - The ticket acceptance criteria define the minimum required outcome

4. **Read supporting artifacts when relevant**
   - Design doc
   - UI preview
   - Pitch
   - Related implementation notes

5. **If the ticket includes frontend work**
   - Locate the relevant design artifact
   - Use it as the visual target
   - The goal is not approximate UI parity — it is faithful implementation of the intended hierarchy, content, states, and behavior

6. **Determine the base branch**
   - Check parent or blocked-by relationships
   - Look for an open branch matching related ticket IDs
   - Use that branch if appropriate for stacked work
   - Otherwise default to `main`
   - If multiple real candidates exist, present them clearly to the user

7. **Create the feature branch**
   - Format:
     - `feat/{LINEAR_ID}-short-description`

## Phase 2: Plan

Break the work into a practical execution plan.

The plan must cover:
- files to create or modify
- dependency order
- schema or migration work first if required
- service or data logic changes
- API changes
- UI work
- tests to add or update
- risk areas

Group work by concern, for example:
- schema / types
- ingestion or processing logic
- API surface
- UI components
- testing

The plan must be implementation-oriented, not abstract.

**Present the plan to the user for approval before writing code.**

This is the scope checkpoint.

## Phase 3: Execute

Implement in the correct order.

### Sequential prerequisites first
Handle anything that other work depends on, such as:
- migrations
- schema updates
- shared types
- enums
- interfaces
- base utility changes

### Then implement feature logic
Build the feature according to the spec and acceptance criteria.

Prioritize:
1. data correctness
2. tenant-safe behavior
3. deterministic logic
4. user-visible functionality
5. polish after correctness

### Frontend implementation rules
If frontend work is involved:
- use the design doc or preview as the source of truth
- match hierarchy, spacing, typography, and state behavior closely
- implement empty, loading, and error states where required
- preserve A.R.I.A.’s product tone:
  - clear
  - operational
  - trustworthy
  - not decorative for its own sake

### Product discipline rules
Do not:
- expand scope because a broader system would be cleaner
- add speculative abstractions for future features
- introduce manual-heavy workflows unless explicitly required
- add AI-like behavior not defined in the spec
- blur exact data with inferred data without explicit guardrails

If implementation reveals a real gap in the spec, flag it clearly.

## Phase 4: Verify

Run the full verification suite required by the repo.

At minimum:
- relevant tests
- linter
- type-checker
- repo-specific verification from CLAUDE.md
- formatting pass if required

If frontend work exists, verify:
- layout fidelity
- correct state handling
- responsive behavior
- no obvious mismatch with the design artifact

If a failure appears:
- fix it directly if straightforward
- otherwise isolate the issue clearly before proceeding

Do not move forward until the implementation is clean and stable.

## Phase 5: Ship

1. **Commit**
   - Use a descriptive commit message referencing the Linear ID
   - Follow CLAUDE.md commit conventions if defined

2. **Push and create the PR**
   - Use `gh pr create`
   - Include:
     - Linear ticket link
     - summary of changes
     - testing notes
     - migration notes if applicable
     - base branch

3. **Prepare PR summary**
   Include:
   - what was built
   - key files or modules changed
   - schema or migration updates
   - test status
   - anything reviewers should pay attention to

4. **Move the Linear ticket to "In Review"**
   - If the workflow and tools support it

## When Things Go Wrong

### Spec not found
Ask the user. Do not guess.

### Design artifact missing for UI-heavy ticket
Ask the user for the correct path or confirm the intended target.

### Acceptance criteria and spec conflict
Pause and surface the conflict clearly.

### Test cannot be satisfied cleanly
Flag the exact mismatch:
- spec issue
- test issue
- implementation misunderstanding

Do not silently work around it.

### Tooling unavailable
Tell the user exactly which tool is missing and what manual step would replace it.

### Ticket is too large
Say so clearly and recommend breaking it into smaller tickets or sub-scopes before implementation.

## A.R.I.A.-Specific Build Priorities

Use these as implementation filters throughout the ticket.

### 1. Financial correctness first
Amounts, aggregations, classifications, and totals must be correct.

### 2. Tenant isolation always
Every query, mutation, and derived output must remain tenant-scoped.

### 3. Trust-sensitive behavior must be careful
Anything touching integrations, sync, financial records, calendar data, or read-only claims must be implemented conservatively.

### 4. Insight systems must be deterministic
If the feature affects derived outputs or insight generation, same input must produce same output.

### 5. Simplicity beats speculative architecture
Build only what the current ticket requires.

## Output Standard

A strong ticket execution should result in:
- the ticket acceptance criteria being satisfied
- the spec being implemented faithfully
- the code fitting the repo patterns
- tests and checks passing
- the PR being reviewable without extra cleanup