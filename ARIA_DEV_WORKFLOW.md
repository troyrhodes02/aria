# A.R.I.A. — Claude Code Workflow

This document outlines the structured workflow for turning a Pitch into implemented, reviewed, and merged production code for A.R.I.A.

The goal is not just to build features — it is to build high-trust, clarity-driven functionality that users will pay for.

---

## 0. When To Use This Workflow

**Use this workflow when:**
- A new Pitch exists in Linear
- You are building a new A.R.I.A. feature
- The work requires structured planning (design → spec → implementation)
- The feature impacts: data structure, UI/UX, insights, onboarding, integrations, or system behavior

**Do not use this workflow for:**
- hotfixes
- small bug fixes
- minor UI tweaks
- refactors without behavior changes

> This workflow is for intentional feature development.

---

## Core Principle: Conditional Skill Usage

Not every step uses every skill.

Each skill should only be used when it meaningfully improves clarity, correctness, or product value.

**Skill Usage Philosophy**
- Use fewer skills, more intentionally
- Avoid generating artifacts that don't add value
- Only introduce complexity when needed

**When to Use Each Skill**

| Skill | Use When |
| --- | --- |
| `/design-doc` | Feature has UI/UX complexity or multiple states |
| `/ui-design` | UI fidelity matters or screen clarity is critical |
| `/freightfi-spec` | Backend, data models, APIs, or system logic are involved |
| `/aria-insight-design` | Feature generates or depends on insights |
| `/aria-onboarding-flow` | Feature affects first-time user experience or activation |
| `/aria-landing-design` | Feature should be reflected on the marketing/landing page |
| `/aria-security-review` | Feature touches sensitive data, integrations, or auth boundaries |
| `/ticket-worker` | Always used for implementation |
| `/review-audit` | Always used after PR review |

---

## 1. Pull the Pitch

**Claude**

```
Pull the Pitch: {Pitch Name} from Linear project A.R.I.A.
```

---

## 2. Design Phase *(Conditional)*

This phase defines what the user sees and experiences.

**Claude — Step 2 *(Optional)***

```
/design-doc add this pitch to the @@pitch/ folder and generate a design doc
```

Use this when: multiple screens exist, interaction complexity exists, or user flow clarity matters.

**Claude — Step 3 *(Optional)***

```
/ui-design generate a pixel perfect design from this design doc
@@design-docs/{pitch-design-doc-name}.md
Store in @@ui-previews/
```

Use this when: visual clarity is critical, layout/state complexity exists, or you need an implementation-ready UI reference.

**Human — Step 4**

Review design artifacts (if created):
- Is the flow intuitive?
- Are states complete?
- Are edge cases covered?
- Does this improve clarity?

> Do not proceed until approved.

---

## 3. Insight / Onboarding / Product Layer *(Conditional)*

These steps ensure the feature delivers actual product value, not just functionality.

**Claude — Step 5 *(Optional)* — Insight Design**

```
/aria-insight-design
```

Use when: feature creates insights, changes how data is interpreted, or impacts what the user learns.

**Claude — Step 6 *(Optional)* — Onboarding Design**

```
/aria-onboarding-flow
```

Use when: feature affects onboarding, impacts time-to-value, or is part of the first-time experience.

**Claude — Step 7 *(Optional)* — Landing Page Update**

```
/aria-landing-design
```

Use when: feature is user-visible and marketable, improves perceived product value, or should be reflected externally.

---

## 4. Technical Specification

**Claude — Step 8**

```
/freightfi-spec
Based on:
  @@design-docs/{design-doc} (if exists)
  @@ui-previews/{ui-preview} (if exists)
  pitch context
```

**Human — Step 9**

Review spec:
- Data model correct?
- API design correct?
- Tenant isolation enforced?
- Edge cases covered?
- Matches product intent?

> Do not proceed until approved.

**Claude — Step 10 *(Optional)* — Security Review**

```
/aria-security-review
```

Use when: feature touches financial data, integrations are involved, auth/permissions are involved, or data exposure risk exists.

---

## 5. Break Into PR-Sized Work

**Claude — Step 11**

Break implementation into PR-sized Linear issues:
- Create milestone: `"{Pitch Name}"`
- Associate all issues with milestone
- Keep tickets small and scoped

**Human — Step 12**

Review tickets:
- Proper scope?
- Clear dependencies?
- Not too large?
- Naming clear?

---

## 6. Implementation

**Claude — Step 13**

```
/ticket-worker work on ARIA-*
```

When done: open PR, use correct base branch, prepare for review.

**Human — Step 14**

Review PR:
- Matches spec?
- Code quality?
- Tests included?
- UI matches design?
- No scope creep?

---

## 7. Review & Audit

**Claude (Codex CLI) — Step 15**

```
/review
```

Review PR changes.

**Claude — Step 16**

```
/review-audit
```

Audit feedback:
- Valid vs invalid
- In-scope vs out-of-scope
- Product-aligned vs misaligned

**Human — Step 17**

Decide:
- What to implement
- What to defer
- What to ignore

If large: create new ticket and add to milestone.

---

## 8. Final Verification Loop

**Human — Step 18**

Repeat Steps 13 → 17 until PR is:
- correct
- clean
- aligned with product intent

---

## 9. Merge

Merge only when:
- spec is satisfied
- product intent is preserved
- no trust/security issues
- no scope creep introduced

---

## Summary Flow

```
Pitch
→ (Optional) Design
→ (Optional) UI
→ (Optional) Insight / Onboarding / Landing
→ Spec
→ (Optional) Security Review
→ Linear Issues
→ Implementation
→ Review
→ Audit
→ Fix
→ Merge
```

---

## Final Principles

### 1. Build for Clarity
Every feature must help users understand their business better.

### 2. Protect Trust
Financial + time data must always be handled safely and accurately.

### 3. Minimize Input
Automation first. User effort second.

### 4. Avoid Scope Creep
If it's not in the pitch/spec, question it.

### 5. Sequence Matters
Build the right things in the right order.
