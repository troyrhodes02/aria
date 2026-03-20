---
name: aria-review-audit
description: >
  Audit PR review feedback for technical validity, product alignment, and
  scope fit against the current A.R.I.A. ticket, spec, and roadmap. Use this
  skill whenever the user asks to evaluate, audit, triage, or assess review
  comments on a pull request — including phrases like "check this review",
  "audit the feedback", "is this review valid", "should I implement this
  feedback", or "scope check this review". Requires Linear MCP to be configured.
---

# Review Audit

Evaluate PR review feedback across three dimensions before recommending action:

1. **Technical validity**
2. **Scope alignment**
3. **A.R.I.A. product fit**

A.R.I.A. is a clarity-first, automation-first product handling financial and optional time data. A review comment is not automatically good just because it is technically possible. It must also fit the current ticket, the current product stage, and the system's trust and simplicity constraints.

## Prerequisites

- Linear MCP server must be connected
- You must be in a git repo with a branch that contains a Linear ticket ID
- The relevant PR comments must be available through:
  - user-pasted feedback
  - GitHub CLI
  - current branch PR context

---

## Workflow

### Step 1: Gather Context

1. **Get the ticket ID** from the current branch name

   ```bash
   git branch --show-current
   ```

2. Extract the ticket ID from the branch name.

3. **Fetch the ticket from Linear** and read:
   - title
   - description
   - acceptance criteria
   - linked issues
   - milestone or parent project
   - any notes that define boundaries

4. **Review related implementation artifacts** when available:
   - pitch
   - design doc
   - spec
   - roadmap entry

5. **Search Linear** for related tickets in the same project to determine whether the feedback belongs to:
   - current scope
   - future planned work
   - a missing follow-up ticket

6. **Get the review comments** from one of:
   - user-pasted feedback
   - PR number via GitHub CLI
   - current checked-out PR

   Example:
   ```bash
   gh pr view --json reviews,reviewRequests,comments
   ```

7. **Read the actual code being commented on.**
   Never evaluate review feedback abstractly without checking the implementation.

---

### Step 2: Evaluate Each Comment

For every review comment, assess these dimensions independently.

#### A. Technical Validity

Ask:
- Is the reviewer's understanding of the code correct?
- Does the issue actually exist in the implementation?
- Would the suggestion improve correctness, reliability, maintainability, or clarity?
- Does the suggestion create new regressions or unnecessary complexity?
- Does it align with the current codebase patterns?

Rate as: `VALID` / `INVALID` / `UNCERTAIN`

---

#### B. Scope Alignment

Using the current ticket, acceptance criteria, and related artifacts:

- **`IN_SCOPE`** — Directly supports the ticket, or fixes a regression introduced by the PR
- **`ALREADY_PLANNED`** — Valid concern, but covered by another existing ticket
- **`NEW_TICKET`** — Valid concern, not currently tracked, worth follow-up
- **`OUT_OF_SCOPE`** — Conflicts with the accepted scope, spec boundaries, or current milestone intent

Scope assessment must reference:
- acceptance criteria
- spec boundaries
- roadmap sequencing
- related ticket IDs when applicable

---

#### C. A.R.I.A. Product Fit

Evaluate whether the feedback supports or harms A.R.I.A.'s product principles.

Ask:
- Does this improve user clarity?
- Does this protect trust with financial or time data?
- Does this preserve automation-first behavior?
- Does this avoid unnecessary manual workflow?
- Does this keep the implementation simple enough for the current product stage?
- Does this introduce speculative complexity that should be deferred?

Rate as: `ALIGNS` / `MISALIGNS` / `UNCERTAIN`

**Examples of misalignment:**
- adding manual-heavy flows where light confirmation is enough
- introducing flexible infrastructure not needed now
- suggesting analytics depth beyond MVP clarity needs
- adding UI/data complexity that does not increase user value

---

### Step 3: Recommend Action

Combine technical validity, scope alignment, and product fit into a final recommendation.

| Validity    | Scope              | Product Fit | → Recommendation                         |
| ----------- | ------------------ | ----------- | ---------------------------------------- |
| `VALID`     | `IN_SCOPE`         | `ALIGNS`    | **IMPLEMENT**                            |
| `VALID`     | `ALREADY_PLANNED`  | `ALIGNS`    | **DEFER**                                |
| `VALID`     | `NEW_TICKET`       | `ALIGNS`    | **DEFER**                                |
| `VALID`     | `OUT_OF_SCOPE`     | `ALIGNS`    | **DEFER**                                |
| `VALID`     | any                | `MISALIGNS` | **SKIP** or **DEFER** depending on impact|
| `INVALID`   | any                | any         | **SKIP**                                 |
| `UNCERTAIN` | any                | any         | **DISCUSS**                              |

**Guidance:**
- **IMPLEMENT** when the suggestion is real, relevant, and helpful now
- **DEFER** when valid but belongs elsewhere or later
- **SKIP** when the comment is wrong, unhelpful, or harmful to the product
- **DISCUSS** when the tradeoff depends on product or architecture judgment not fully resolved in context

---

### Step 4: Present the Audit

Format the output as a clear, scannable report.

For each comment:

```markdown
## [RECOMMENDATION] — one-line summary
📎 path/to/file.ts:L42

**Reviewer said:** "brief quote or paraphrase"

**Technical assessment:** [VALID / INVALID / UNCERTAIN]
[1–2 sentences of technical analysis]

**Scope assessment:** [IN_SCOPE / ALREADY_PLANNED / NEW_TICKET / OUT_OF_SCOPE]
[1–2 sentences with references to ticket acceptance criteria, spec, or roadmap]

**Product fit:** [ALIGNS / MISALIGNS / UNCERTAIN]
[1–2 sentences explaining whether this supports A.R.I.A.'s clarity, trust, simplicity, and automation goals]

**Recommendation:** [What to do and why]
```

End with a summary count:
- X to implement
- X to defer
- X to skip
- X to discuss

---

## Important Guidelines

- Always read the actual code
- Always compare against the current ticket
- Use the spec and design doc when available
- Cite specific acceptance criteria when classifying scope
- Cite specific ticket IDs when something is already planned
- Be honest when uncertain
- Do not implement anything
- Default stylistic-only feedback to **SKIP** unless it materially improves maintainability or clarity
- Default speculative architecture expansion to **SKIP** or **DEFER**
- Protect A.R.I.A. from scope creep disguised as code quality

---

## A.R.I.A.-Specific Review Heuristics

Use these as additional filters when auditing feedback.

### 1. Clarity Filter
If the suggestion does not improve the system's clarity, trust, or implementation quality, be skeptical.

### 2. Automation Filter
If the suggestion introduces unnecessary manual workflow, it likely does not fit.

### 3. Trust Filter
If the comment identifies a real data integrity, auth, tenant isolation, or sensitive-data risk, prioritize it highly.

### 4. Scope Discipline Filter

If the suggestion sounds like:
- *"while we're here"*
- *"this will help later"*
- *"we might as well"*
- *"let's make this flexible"*

treat it as a likely scope expansion and audit aggressively.

### 5. MVP Filter
Prefer the smallest correct implementation that supports current product value. Do not recommend complexity simply because it is architecturally cleaner in the abstract.
