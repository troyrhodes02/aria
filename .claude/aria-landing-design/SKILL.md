---
name: aria-landing-design
description: Design and update the A.R.I.A. landing page in a way that reflects the real state of the product. Use when creating the initial marketing site, refining messaging, adding new product sections, or updating the landing page to reflect newly completed features. The landing page must position A.R.I.A. clearly, build trust, and only make claims supported by what exists in the product.
---

# A.R.I.A. Landing Design

Design and update the A.R.I.A. landing page as a product-positioning asset, not just a marketing page.

The landing page exists to do four things well:

1. Explain what A.R.I.A. does in direct, credible language
2. Make the value of the product understandable within seconds
3. Build trust around connecting financial and calendar data
4. Evolve as the product evolves, without overclaiming

A.R.I.A. is not a vague “AI business assistant.” It is a clarity product for small operators and teams that want to understand how their business makes money — and, when available, what their time is worth — without manual tracking.

The landing page must always feel:
- sharp
- trustworthy
- financially literate
- operationally useful
- grounded in real functionality

## Core Rule

The landing page must only describe:
- what already exists
- what can be demonstrated
- what can be explained honestly

Do not market unfinished capabilities as if they are working.
Do not imply intelligence the product does not yet have.
Do not use vague aspirational copy when specific operational wording would be clearer.

## Purpose of the Landing Page

The landing page should help a visitor quickly answer:

- What is this?
- Who is it for?
- What problem does it solve?
- Why is it different from spreadsheets or accounting tools?
- Why should I trust it with my data?
- Why should I care now?

## Output Requirements

When generating or updating the landing page, provide:

1. **Page strategy summary**
   - What stage of product maturity this page reflects
   - What the page is trying to achieve
   - What visitor questions it prioritizes

2. **Section-by-section structure**
   - Hero
   - Supporting proof blocks
   - Product explanation
   - Feature/value sections
   - Trust/security messaging
   - CTA structure
   - FAQ if needed

3. **Copy recommendations**
   - Headlines
   - Subheads
   - CTA labels
   - Body copy
   - Trust messaging

4. **Visual direction**
   - Layout hierarchy
   - Product preview usage
   - Screenshot or interface placement
   - Editorial pacing between sections

5. **HTML implementation**
   - Single-file HTML
   - Full document with `html`, `head`, and `body`
   - Responsive layout
   - Functional and detailed enough to use as a working landing page draft

## Implementation Rules

When producing the landing page implementation:

- Output only one code block for the page code
- Use a single-file HTML document
- Always include:
  - `html`
  - `head`
  - `body`
- Use the Archivo font from Google Fonts
- Use lucide icons via JavaScript when icons are needed
- Use stroke width `1.5`
- Keep the implementation responsive
- Use direct, production-lean structure rather than throwaway mock markup
- Use hover, divider, outline, and spacing treatments intentionally
- Prefer typographic/data-driven visual sections over generic marketing illustration

## Brand System

Use the current A.R.I.A. brand language reflected in the product and screenshots.

### Primary Font
- Archivo

### Color Palette
- Off-White: `#F9FAFB`
- Concrete Grey: `#F5F6F7`
- Charcoal: `#1E1E1E`
- Primary Accent Blue: `#2563EB`
- Mint Green: `#A7F3D0`
- Deep Accent: `#1E3A8A`
- A.R.I.A. Purple Accent: `#4639CA`

### Usage Guidance
- Off-White for page background
- Concrete Grey for cards, surfaces, dividers, and subtle panels
- Charcoal for primary headings and core text
- Blue for primary CTAs, links, and active emphasis
- Mint Green for positive states, freshness accents, subtle supporting indicators
- Deep Accent for darker emphasis and higher-contrast hover states
- Purple Accent may be used where it matches the actual A.R.I.A. UI and identity shown in product references

Do not introduce unrelated colors.
Avoid warm palettes, hype gradients, or consumer-SaaS softness.

## Aesthetic Direction

The page should feel like:
- a financial intelligence product
- a serious workflow tool
- a modern software company with editorial restraint

The tone should be:
- direct
- operational
- confident
- specific

Not:
- fluffy
- inspirational for its own sake
- over-designed
- template-like

## Layout Rules

- Use strong visual hierarchy
- Left-align headlines by default
- Centered hero text is allowed only when brief and visually controlled
- Avoid repetitive three-card feature grids as the default structure
- Use asymmetry, staggered sections, or editorial stacking
- Use whitespace aggressively
- Let screenshots and copy breathe
- Use section transitions that feel deliberate, not blocky
- Avoid giant colored hero backgrounds
- Avoid fake dashboard placeholders
- If screenshots are available, use them
- If screenshots are not available, use typographic or data-driven visual structures

## Product Truthfulness Rules

The landing page must reflect the current reality of A.R.I.A.

### Early Stage / Pre-MVP
Emphasize:
- the core promise
- what the product is being built to do
- demo / waitlist / early access CTA
- trust and positioning

Do not imply:
- broad feature completeness
- automated depth that does not exist yet
- polished live workflows if they are not implemented

### Mid-Build
Add:
- real product sections
- feature blocks tied to shipped work
- screenshots or UI previews from actual interfaces
- copy that reflects what users can now meaningfully experience

### MVP-Ready
Expand into:
- feature detail
- product proof
- stronger feature-led CTA structure
- clearer comparison against current alternatives

## Content Priorities

The landing page should communicate these ideas with precision:

### 1. Core Value Proposition
A.R.I.A. helps users understand how their business makes money without manual analysis.

### 2. Time Value Layer
When calendar/time capability is relevant and implemented, explain that A.R.I.A. can help users understand what their time is worth.

Do not force this message if the product state does not support it yet.

### 3. Revenue Entity Clarity
Explain that the product goes beyond raw transactions and helps users see the real drivers of revenue.

### 4. Minimal Effort
Position the system as low-input and automation-first.

### 5. Trust
Be clear about:
- read-only data handling
- no movement of funds
- no calendar modification
- practical, serious data handling language

## Required Landing Sections

Every landing page draft should evaluate and usually include the following sections, unless there is a clear reason not to.

### 1. Hero
Must communicate:
- what A.R.I.A. is
- who it is for
- what clarity it provides
- a specific CTA

### 2. Product Framing
Explain what problem A.R.I.A. solves better than:
- spreadsheets
- accounting dashboards
- fragmented payment platforms

### 3. Visual Product Proof
Use:
- actual product screenshots
- cropped interface proof
- real UI callouts
- focused previews of insights, dashboards, or entity views

### 4. How It Works
Keep this simple and operational:
- connect accounts
- A.R.I.A. structures activity
- A.R.I.A. surfaces what matters

### 5. Value Blocks
Show specific user outcomes such as:
- see where revenue comes from
- identify concentration risk
- understand time versus return
- surface important changes automatically

Only include blocks supported by the actual product stage.

### 6. Trust / Security
Explicitly reinforce:
- read-only access
- financial data sensitivity awareness
- practical trust messaging

### 7. CTA Section
CTA language should be specific:
- Book Demo
- Join Waitlist
- Start Free Trial
- Get Early Access

Choose the CTA that matches the product stage.

## Copy Rules

### Headlines
- short
- direct
- specific
- high signal

### Subheads
- explain the product in operational terms
- avoid inflated promises

### Body Copy
- explain outcomes clearly
- reduce abstraction
- avoid “transform your business” style fluff

### CTA Copy
Use specific action language.

Preferred examples:
- Book Demo
- See the Product
- Start Free Trial
- Join Early Access

Avoid vague CTA language when a more concrete option exists.

## Screenshot Usage Rules

If screenshots are provided:
- use them as primary proof
- preserve their credibility
- crop or frame them cleanly
- do not distort them into decorative filler

If multiple screenshots are provided:
- assign each to a clear purpose
- hero preview
- product proof
- feature section support

Do not overload the page with too many UI images at once.

## Section Writing Rules

For each section, define:
- what the visitor should understand
- what proof supports it
- what action or trust signal follows

Every section must earn its place.
Do not add filler sections simply because a landing page “usually has them.”

## FAQ Rules

Add FAQ only when it improves trust or reduces friction.

Good FAQ topics:
- What data does A.R.I.A. access?
- Does it move money?
- Do I need to manually track time?
- Who is this for?
- What integrations are supported?

## Tone Rules

Write like a product that understands operations and money.

Good tone:
- “See which clients actually drive your revenue.”
- “Understand what your time is worth without manual tracking.”
- “Connect Stripe, PayPal, and your bank accounts to see the full picture.”

Bad tone:
- “Unlock your business potential with AI-powered transformation.”
- “Supercharge your workflow with intelligent automation.”
- “Reimagine the future of your business.”

## Code Generation Rules

When generating the page code:
- produce a complete single-file HTML page
- keep all implementation inside one code block
- use only the approved visual language
- avoid unnecessary JavaScript
- use JavaScript only where needed, including lucide icon initialization and chart behavior if charts exist
- if charts are included, use Chart.js
- keep structure clean and implementation-ready

## Product Integrity Rules

Do not:
- invent supported integrations that do not exist
- imply live sync if only simulation exists
- imply advanced AI reasoning if the system is rule-based
- market future roadmap items as current product capability
- use testimonials, logos, or trust marks without real basis

## Workflow

1. Review the current state of the product or pitch
2. Identify what is real, what is demoable, and what should remain unclaimed
3. Decide the landing page’s primary goal for this stage
4. Structure the page around that goal
5. Write direct, product-truthful messaging
6. Use screenshots or UI proof where available
7. Add trust language where hesitation is likely
8. Generate the implementation in a single HTML file

## Quality Standard

A strong A.R.I.A. landing page should make a qualified visitor think:

- I understand what this does
- This seems useful
- This feels credible
- I trust how it handles data
- I want to see more

If the page feels generic, overclaims the product, or could describe ten other SaaS tools, it is not good enough.