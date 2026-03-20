---
name: aria-design-doc
description: Generate design documents for A.R.I.A. features. Use when creating UI/UX specifications, screen-by-screen designs, or when the user mentions "design doc", "design document", "PRD", or needs to document how a feature should look and behave. Design docs define the visual and interaction layer — what users see, how they interact, how screens connect, and how the feature delivers clarity. Requires a pitch document as input for scope, value, and problem context.
---

# A.R.I.A. Design Document Generator

Generate design documents that specify the UI/UX experience for A.R.I.A. features. Design docs sit between pitch documents (problem, value, scope, monetization) and technical specs (data models, logic, APIs, jobs). They define what the user sees, how they interact, and how the product delivers visible clarity.

A.R.I.A. is not a generic dashboard product. It is a system that helps users understand how their business makes money — and, when available, what their time is worth — with minimal manual effort. Every design decision must reinforce clarity, visibility of insight, simplicity, and trust.

## Where Design Docs Fit

```text
Pitch Document → Design Document → Technical Spec
(why + scope)    (what users see)   (how it's built)

Pitch answers: Why are we building this? What user problem does it solve? Why would someone pay for it? What is in scope?

Design Doc answers: What does the user see? How do they move through it? What states exist? Where is the value surfaced?

Spec answers: How is it implemented? What is the data model? What logic powers it? How do we test it?
```

## Prerequisites

Before writing a design doc, ensure you have:

- **Pitch document** — Required. Defines the problem, clarity impact, monetization impact, scope, no-gos, and feature boundaries.
- **Project design context / style guide** — If an A.R.I.A. style guide exists, reference it. If not, preserve consistency with the current product visual language and document only what this feature specifically uses or introduces.
- **A.R.I.A. product context** — The feature must align with the product's core principles:
  - automation first
  - minimal user input
  - insights over raw data
  - clarity over complexity
  - trust for financial and calendar data
- **Related artifacts when applicable** — Existing pitches, roadmap entries, feature breakdowns, or current UI references

## File Conventions

- **Filename:** `feature-name-design-doc.md` (kebab-case)
- **Location:** Project docs directory or as specified by user
- **Format:** Markdown with JSX/Tailwind code blocks for component specifications and ASCII wireframes for layout

## Core Design Standard for A.R.I.A.

Every design doc must optimize for these questions:

- What becomes clearer for the user because this exists?
- Where is the value surfaced visually?
- How quickly does the user feel the benefit?
- Does this reduce thinking, or add thinking?
- Does this increase trust in the product?

A screen is not complete just because it looks correct. It is complete when:

- the hierarchy is obvious
- the important information is noticeable
- the user knows what the system is telling them
- the next action is clear
- unnecessary interpretation is removed

---

## Required Sections

Every design doc must include these sections in order:

### 1. Title & Metadata

```markdown
# Feature Name — Design Document

**Version:** X.Y
**Focus:** One-line description of what this design doc covers
```

---

### 2. Vision

2–4 sentences maximum. Frame the feature from the user's perspective — what business question does this page, flow, or module answer? What becomes easier to understand? End with a design north star that captures the interaction and visual philosophy for this feature.

**Example:**

> Users should not have to interpret disconnected transactions to understand what is driving their business. This feature makes revenue structure visible by turning noisy financial activity into a clear, ranked entity view that shows what matters most and why.
>
> Design north star: Calm financial clarity. High-signal hierarchy, low-friction interaction, and obvious insight visibility.

**Guidelines:**
- Lead with user understanding, not system behavior
- Frame around business clarity, not just workflow
- Make the north star specific and opinionated
- Reinforce A.R.I.A.'s product identity: intelligent, simple, trustworthy, low-effort

---

### 3. Design Principles

3–5 numbered principles, each with a short name and 1–2 sentence explanation. These must be specific to this feature, not generic design advice.

**Format:**

```markdown
### 1. [Principle Name]

[What it means and how it affects design decisions for this feature.]
```

**Examples of strong A.R.I.A.-style principles:**
- Surface the answer first
- Money before mechanics
- Time only when trustworthy
- Insight must feel noticeable
- Confirmation without friction

**Guidelines:**
- Each principle should resolve a real design tension
- Principles should help implementation tradeoffs
- Ground them in the pitch's problem, clarity impact, and monetization impact
- Keep to 3–5 total

---

### 4. Visual Language

Reference the A.R.I.A. product visual language and specify any feature-specific tokens or interaction decisions.

Include:

**Color Palette**

Table of design tokens used in this feature with hex values and usage descriptions.

**Severity / Status Colors**

If the feature includes risk states, confidence levels, sync states, or insight priority levels, define the color mapping clearly.

Examples:
- Positive trend
- Warning / incomplete data
- High-priority insight
- Read-only trust messaging
- Missing mapping / unmatched data

**Typography**

Table of text elements with size, weight, line-height, and Tailwind class examples.

**Spacing**

Reference the spacing system in use. Document any feature-specific spacing rules that improve scan-ability and hierarchy.

**Border Radius / Surface Language**

Document panel/card/button treatment used by this feature. If following an existing style guide, state that directly and only note deviations.

> **Important:** Always include:
> *All styling inherits from the A.R.I.A. design system or current product visual language. This design doc documents only the subset of tokens, hierarchy rules, and interaction patterns actively used by this feature.*

**Guidelines:**
- Do not invent unnecessary new tokens
- Prefer consistency over novelty
- Financial clarity surfaces should feel calm, structured, and trustworthy
- High-value information should be visually prominent without becoming noisy

---

### 5. Information Architecture

ASCII diagram showing the page hierarchy and navigation relationships. This gives a top-level view of how the feature fits into the product.

**Example:**

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar                                                        │
│ ├─ Dashboard                                                   │
│ ├─ Revenue Entities                                            │
│ ├─ Insights                                                    │
│ └─ Settings                                                    │
│                                                                 │
│ Main Content                                                    │
│ ┌─ Summary Module ────────────────────────────────────────────┐ │
│ │ Key metric / primary answer                                 │ │
│ └─────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌─ Supporting Section ────────────────────────────────────────┐ │
│ │ Ranked list / chart / detail / state explanation            │ │
│ └─────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌─ Secondary Detail / Action Area ────────────────────────────┐ │
│ │ Optional review, mapping, or drill-down                     │ │
│ └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

**Guidelines:**
- Show where the feature lives in A.R.I.A. navigation
- Show primary vs secondary content clearly
- Show how insight surfaces connect to detail views when relevant
- Keep it simple and structural, not pixel-perfect

---

### 6. Screen Specifications

This is the core of the design doc. Each screen gets its own section using the same structure.

**Per-Screen Structure:**

```markdown
## Screen N: [Screen Name]

### Purpose

One sentence: what does this screen help the user understand or do?

### URL Pattern (if applicable)

`/feature/[id]`

### Trigger (for panels/modals/drawers)

What user action opens this screen/panel?

### Primary User Question

What question should this screen answer immediately?

### Layout

ASCII wireframe showing the spatial arrangement of elements.

### [Component / Section Name]

| Element          | Styling & Behavior                  |
| ---------------- | ----------------------------------- |
| **Element name** | Tailwind classes, hierarchy notes, interaction behavior |

### Code Reference

​```jsx
<div className="...">
  {/* Component structure */}
</div>
​```

### Fields (for forms/review flows)

| Field          | Type       | Required | Notes                |
| -------------- | ---------- | -------- | -------------------- |
| **Field Name** | Input type | Yes/No   | Validation, defaults |

### Validation

- Validation rules
- Disabled states
- Confirmation states
- Error message patterns

### Empty State

ASCII wireframe + JSX for when there is no data.

### Behavior

- What happens on success
- What happens on error
- Toast messages
- Focus management
- Transition behavior
- What changes visually after key actions
```

**Screen Specification Guidelines:**
- Every screen needs a Purpose
- Every screen needs a Primary User Question
- ASCII wireframes before code
- Every meaningful section needs a styling/behavior table
- Empty states are required
- Interactions must be explicit
- Review flows and confirmation flows must be lightweight
- If the screen shows insights, define exactly where the insight appears and why it is noticeable
- If the screen includes time-based information, only surface it when data reliability is sufficient
- Avoid creating "dashboard clutter" sections that add data but not understanding

---

### 7. Navigation Flows

Document how screens connect to each other and to other parts of A.R.I.A. Use ASCII flow diagrams showing the user journey.

**Example:**

```
┌─ Dashboard ─────────────────────────────┐
│  [Top Revenue Entities]                 │
│  click entity row ──────────────────────│──→ Revenue Entity Detail
└─────────────────────────────────────────┘

┌─ Onboarding Review ─────────────────────┐
│  [Confirm mappings]                     │
│  continue ──────────────────────────────│──→ Dashboard
└─────────────────────────────────────────┘
```

Include:
- What triggers navigation
- Whether it is click, keyboard, step progression, or automated transition
- What state carries between screens
- Deep-link patterns where applicable
- Whether the flow is first-session only, recurring, or both

---

### 8. Interaction Specifications

**Keyboard Navigation**

| Context     | Key | Action       |
| ----------- | --- | ------------ |
| Screen Name | Key | What it does |

**Loading States**
- Skeleton patterns
- Staged loading behavior
- Sync / ingestion progress states where relevant
- How anticipation and trust are maintained during loading

**Error States**

Provide JSX for error display and retry patterns.

**Empty States**

For every empty state, define:
- what it means
- whether it is expected or problematic
- what action or explanation follows

**Toast Notifications**

| Action         | Message                      | Duration |
| -------------- | ---------------------------- | -------- |
| Success action | "Confirmation text"          | 3s       |
| Error          | "Error text. Try again."     | 5s       |

**Trust Messaging**

If the feature touches financial data, calendar data, integrations, permissions, or review flows, explicitly define trust-supporting microcopy such as:
- read-only reassurance
- sync explanation
- why data is being shown
- what is exact vs what may require confirmation

> This is required when trust influences user willingness to continue.

---

### 9. Responsive Behavior

**Breakpoints**

| Breakpoint | Width      | Behavior                   |
| ---------- | ---------- | -------------------------- |
| Desktop    | ≥1024px    | Full layout as specified   |
| Tablet     | 768-1023px | Adaptations                |
| Mobile     | <768px     | Mobile-specific behavior   |

**Mobile Adaptations**

Document:
- stacking behavior
- panel/drawer conversions
- interaction changes
- touch target rules
- priority of information on small screens

**Guidelines:**
- Preserve hierarchy, not exact layout
- Do not let mobile hide the most valuable information
- Keep critical metrics and insights visible quickly

---

### 10. Component Inventory

| Component       | Location        | Notes                              |
| --------------- | --------------- | ---------------------------------- |
| ComponentName   | Which screen(s) | New vs reusable, key variants      |

**Guidelines:**
- Note whether the component is A.R.I.A.-specific or reusable system UI
- Flag components that surface insight, trust, or review states
- Keep component count lean to avoid overbuilding

---

### 11. Out of Scope (v1)

Bullet list of features explicitly not designed in this version. Reference the pitch document's no-gos and feature boundaries.

Examples:
- no advanced analytics
- no manual tracking tools
- no predictive recommendations
- no extra admin controls unless required
- no alternate views not necessary for value delivery

> This section is mandatory. It protects MVP clarity and prevents scope creep.

---

## Style Guidelines

### ASCII Wireframes
- Use box-drawing characters (`┌ ─ ┐ │ └ ┘ ├ ┤`) for wireframes
- Focus on layout hierarchy and relationships
- Label interactive elements with `[brackets]`
- Use `→` for links or directional flow
- Include populated and empty states when relevant

### Code Blocks
- Use JSX + Tailwind
- Match the codebase conventions in use
- Include interaction states where relevant
- Use conditional class handling when needed
- Prefer practical implementation-ready snippets over decorative mock markup

### Tables
- Use markdown tables for element styling, fields, keyboard shortcuts, and state definitions
- Bold the first-column names
- Keep entries implementation-relevant

### Tone
- Write descriptions in plain English
- Focus on what the user understands, sees, and can do
- Be explicit about success, failure, ambiguity, and trust-sensitive moments
- Prioritize clarity over design jargon

---

## A.R.I.A.-Specific Design Rules

These rules apply to every A.R.I.A. design doc:

### 1. Clarity Comes Before Density
Do not add charts, metrics, or modules unless they improve understanding. More data is not better.

### 2. Insight Must Be Visible
If the feature creates value through insight, the insight must be visually noticeable and easy to interpret.

### 3. Money First, Time Second
If both financial and time data appear, financial clarity remains primary unless the feature specifically exists to answer a time-efficiency question.

### 4. Trust Must Be Designed
If the user could hesitate because of data sensitivity, permissions, sync behavior, uncertainty, or inferred mapping, the interface must explain enough to maintain confidence.

### 5. Manual Work Must Stay Minimal
Avoid workflows that require repeated correction, heavy configuration, or unnecessary forms. Confirmation is acceptable. Maintenance-heavy UX is not.

### 6. No Fake Sophistication
Do not design interfaces that imply AI depth, precision, or analytical certainty the system does not actually have.

---

## Workflow

1. Receive pitch document
2. Read the problem, clarity impact, monetization impact, no-gos, and scope boundaries
3. Identify user-visible surfaces — screens, panels, modules, onboarding steps, states
4. Identify the primary user questions — what should become obvious on each screen?
5. Clarify only when needed — ask questions only when ambiguity materially affects layout, hierarchy, trust, or scope
6. Draft information architecture
7. Design screen-by-screen
8. Add interaction specs — loading, empty, error, responsive, trust states
9. Document component inventory
10. Checkpoint with user when appropriate — confirm hierarchy, clarity, and scope
11. Finalize — confirm this design aligns with the pitch and does not overreach

---

## Interview Guidelines

When clarifying with the user:

- Use existing A.R.I.A. project context first before asking questions
- Ask only when the answer materially changes the design
- Prioritize questions about:
  - what must be most visible
  - what the user should understand immediately
  - whether this belongs in onboarding, dashboard, insight feed, or entity detail
  - what trust/risk states need explanation
  - what should remain out of scope
- Avoid asking questions already answered by the pitch or product docs
- Keep questions minimal and high leverage
- Prefer 2–5 strong questions over long interviews

---

## Output Quality Standard

A strong A.R.I.A. design doc should make it obvious:

- what the feature looks like
- what the user sees first
- what becomes clearer because of it
- where the value appears
- what states exist
- what is intentionally left out

> If the design doc only describes layout but does not describe how the feature delivers clarity, it is incomplete.
