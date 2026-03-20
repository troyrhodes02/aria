---
name: aria-insight-design
description: Generate structured insight definitions for A.R.I.A. features. Use when a feature creates, modifies, prioritizes, or depends on business insights derived from financial data, time data, revenue entities, or their relationships. This skill defines what the insight is, why it matters, what triggers it, how it should be explained, and how it should be surfaced so the product delivers real clarity rather than raw data.
---

# A.R.I.A. Insight Design

Generate structured insight definitions for A.R.I.A. features.

A.R.I.A. is not a dashboard-first product. It is a clarity engine. This skill exists to make sure features that depend on insights are designed as actual insight systems — not just metrics, charts, or loosely interpreted observations.

An insight is only valid if it helps the user understand something meaningful about how their business makes money, what is changing, what is risky, or what their time is worth.

## Purpose

Use this skill to define:

- what the insight actually says
- what user question it answers
- what data it depends on
- what exact conditions trigger it
- how it should be prioritized
- how it should be explained
- what action or implication it suggests
- where it should surface in the product

The output should be detailed enough to inform:
- pitch writing
- design docs
- technical specs
- rule implementation
- UI copy
- acceptance criteria

## Core Insight Standard

Every insight in A.R.I.A. must satisfy all of the following:

1. **Clarity**
   - It makes something meaningfully easier to understand

2. **Truthfulness**
   - It is based on real, explainable underlying data
   - It does not imply certainty beyond what the data supports

3. **Actionability**
   - It implies a useful next thought, question, or decision

4. **Visibility**
   - It can be surfaced in a way the user will actually notice

5. **Retention Value**
   - It contributes to why the product is worth returning to

If an insight fails one of these, it should be revised or rejected.

## What Counts as an Insight

Good insight types for A.R.I.A. include:
- concentration risk
- growth or decline
- unusual changes
- recurring expense patterns
- unmatched transaction warnings
- high-time / low-return warnings
- effective hourly value comparisons
- payment irregularity
- time-to-revenue imbalance

These are insights because they interpret data into something meaningful.

## What Does NOT Count as an Insight

These are not insights on their own:
- total revenue
- number of clients
- list of transactions
- chart without explanation
- rank without implication
- raw metric with no threshold or interpretation

If it is only data display, it is not enough.

## Prerequisites

Before generating insight definitions, review:

1. **Pitch or feature description**
   - what problem the feature solves
   - why it matters
   - what is in scope

2. **Related design or product context**
   - where this insight will surface
   - what the user sees before and after

3. **Relevant system context**
   - available data sources
   - reliability constraints
   - existing insight patterns if they exist

## Output Structure

For each insight, include the following sections in order.

---

## 1. Insight Name

A concise, product-usable name.

Examples:
- Revenue Concentration Risk
- Declining Revenue Trend
- High-Time / Low-Return Warning
- Recurring Expense Detected

---

## 2. Insight Purpose

One short paragraph covering:
- what this insight helps the user understand
- why that understanding matters
- what business question it answers

Example:
> This insight helps the user see when too much revenue depends on a single entity. It matters because a business can appear healthy while still being fragile. It answers the question: “Am I overly dependent on one client, product, or source?”

---

## 3. Core User Question

State the exact question this insight answers.

Examples:
- “Am I too dependent on one client?”
- “Is my revenue growing or slipping?”
- “Am I spending a lot of time on low-value work?”
- “What expenses keep recurring without me noticing?”

This section is mandatory.

---

## 4. Why This Is Valuable

Explain:
- what becomes clearer because this insight exists
- why a user would care
- why this helps justify ongoing product value

Cover both:
- **Immediate value**
- **Ongoing value**

---

## 5. Required Data Inputs

List the exact data required.

Example format:

| Input | Required | Notes |
|-------|----------|------|
| Revenue events | Yes | Exact amounts and dates |
| Revenue entity assignments | Yes | Needed for grouping |
| Calendar time events | Optional | Only for time-based variants |
| Expense events | No | Not used |

Be explicit about minimum viable data.

---

## 6. Trigger Logic

Define the exact conditions under which the insight appears.

Use precise, deterministic language.

Example:
- Generate when one revenue entity contributes more than 40% of total revenue over the last 90 days
- Generate when current 30-day revenue is at least 20% lower than previous 30-day revenue
- Generate when mapped time share exceeds revenue share by at least 25 percentage points

Include:
- thresholds
- date windows
- comparison windows
- fallback behavior
- minimum sample sizes

This is one of the most important sections.

---

## 7. Non-Trigger Conditions

Define when the insight must NOT appear.

Examples:
- Do not generate if fewer than 3 revenue events exist
- Do not generate if time mapping confidence is weak
- Do not generate if the comparison period is incomplete
- Do not generate if the difference is too small to matter

This protects trust and prevents noisy insight feeds.

---

## 8. Explanation Logic

Define how the system should explain the insight once triggered.

Include three layers:

### Statement
The short headline version.

### Explanation
The plain-language interpretation.

### Implication / Suggested Action
What the user should think about next.

Example:

**Statement:**  
Client Acme generated 47% of your revenue in the last 90 days.

**Explanation:**  
Nearly half of your recent revenue came from a single client, which increases concentration risk if that relationship changes.

**Implication / Suggested Action:**  
Consider whether you are comfortable with that dependency or whether you should diversify revenue sources.

All wording should be:
- direct
- non-hyped
- trustworthy
- specific

---

## 9. Priority Logic

Define how important this insight is relative to others.

Include:
- what makes it high, medium, or low priority
- whether severity increases with stronger thresholds
- tie-breaking rules if needed

Example:
- High priority if concentration exceeds 50%
- Medium priority if between 35% and 50%
- Low priority if below 35%, or do not show at all

---

## 10. Trust Boundaries

Define what must be true for this insight to remain trustworthy.

Include:
- what data is exact
- what is inferred
- what could be misleading if shown incorrectly
- what should never be approximated

Examples:
- Revenue totals must be exact
- Time-based interpretation must not appear if mapping is uncertain
- Anomaly claims must be threshold-based, not subjective

This section is mandatory.

---

## 11. Surface Recommendation

Define where this insight should appear.

Possible surfaces:
- Dashboard
- Insight Feed
- Revenue Entity Detail View
- Onboarding summary
- Review flow follow-up

Also define:
- visibility level
- whether it should appear as a card, highlight, alert, or inline module
- whether it deserves first-session exposure

---

## 12. UX Notes

Define what the UI must do to make this insight effective.

Examples:
- Keep the statement visible above supporting numbers
- Do not hide the explanation behind a click
- Show the compared periods directly in the card
- Include a link to the relevant entity when possible

Focus on clarity and noticeability.

---

## 13. Edge Cases

List realistic cases the system must handle.

Examples:
- Revenue is concentrated because the dataset is still small
- One-time spikes distort trend comparisons
- Time data exists but has weak mapping
- Entity rename affects continuity
- Missing historical data makes comparison unstable

---

## 14. Acceptance Criteria

Use a checklist format.

Example:

- [ ] Insight only appears when trigger conditions are met
- [ ] Insight does not appear when sample size is too small
- [ ] Statement uses exact underlying values
- [ ] Explanation matches the actual condition detected
- [ ] Surface location is defined
- [ ] Priority logic is defined
- [ ] Trust boundaries are explicit

---

## 15. Explicit Non-Goals

List what this insight is not trying to do.

Examples:
- ❌ Predict future revenue
- ❌ Provide prescriptive business advice
- ❌ Use AI inference
- ❌ Explain root cause beyond available data
- ❌ Replace detailed analysis tools

This keeps the insight honest and in scope.

## Insight Writing Rules

### 1. Use Business Language, Not Internal Logic Language

Write:
- “A large share of your revenue comes from one client”

Not:
- “Entity contribution exceeded concentration threshold”

### 2. Be Specific

Use exact time windows, thresholds, and conditions.

### 3. Avoid False Precision

Do not imply certainty beyond the input data.

### 4. Avoid Alarmism

Insights can be important without sounding dramatic.

### 5. Prefer Interpretation Over Decoration

The value is in the meaning, not the styling.

## A.R.I.A.-Specific Rules

### Financial Insights
Must be based on exact financial calculations.

### Time-Based Insights
Must only appear when time data is reliable enough to support interpretation.

### Entity Insights
Must reflect the current revenue-entity model, not assume a pure client-only business structure.

### MVP Discipline
Prefer simple, rule-based, believable insight logic over sophisticated but fragile logic.

## Workflow

1. Read the feature or pitch context
2. Identify whether the feature creates or depends on an insight
3. Define the core user question
4. Define the required data inputs
5. Define trigger and non-trigger logic
6. Define statement, explanation, and implication
7. Define priority and trust boundaries
8. Define surface recommendation
9. Add edge cases and acceptance criteria
10. Reject or refine any insight that is weak, noisy, or misleading

## Quality Standard

A strong A.R.I.A. insight definition should make it obvious:
- what the insight is
- why it matters
- when it appears
- when it must not appear
- what data it relies on
- how it should be explained
- how it contributes to product value

If the output is just a metric description or vague observation, it is not good enough.