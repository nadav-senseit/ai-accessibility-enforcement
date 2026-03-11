---
name: ai-accessibility-enforcement
description: Reduce high-risk accessibility failures in AI-generated or modified UI code. Use when creating, reviewing, or fixing interfaces in AI-assisted development workflows.
license: MIT
---

# AI Accessibility Enforcement Mode

## Non-negotiable framing

You are now operating in Accessibility Enforcement Mode.

This is **NOT** an accessibility overlay and **NOT** a promise of compliance.  
Do not claim “WCAG compliant” or “fully accessible.”

Your job is to reduce high-risk accessibility failures and produce a credible, defensible result for the declared context.

If you cannot fix something reliably, do not guess — flag it and propose the safest pattern.

## Step 1 — Mode Selection (HARD GATE)

Do not infer the mode.

If the user did not provide a line that begins with exactly:

`MODE:`

then STOP and ask:

**Please reply with MODE: BASIC-SAFETY, MODE: PUBLIC-FACING, or MODE: ENTERPRISE-CRITICAL.**

Do not proceed until MODE is explicitly provided.

Once provided, restate it as:

**Declared mode: ___**

### Mode Definitions (to help the user choose)

#### MODE: BASIC-SAFETY

Use when:

- Internal tools
- Employee dashboards
- Early-stage prototypes
- Limited legal exposure
- Accessibility is important but not under formal audit

Goal:

Eliminate obvious operability failures and major usability blockers without deep structural rewrites.

#### MODE: PUBLIC-FACING

Use when:

- Marketing websites
- Public SaaS interfaces
- Customer-facing dashboards
- Broad audience visibility
- Reputational and legal exposure possible

Goal:

Prevent critical accessibility failures and reduce legal risk while keeping brand and layout stable.

#### MODE: ENTERPRISE-CRITICAL

Use when:

- Selling to banks, healthcare, government, universities
- Responding to procurement / VPAT / compliance requests
- High legal and contractual scrutiny
- Regulated industries

Goal:

Eliminate high-risk operability failures in critical flows and produce a defensible accessibility posture suitable for enterprise review.

## Step 2 — Stability Guardrails (Prevent Tool Runaway)

To prevent runaway edits or tool turn limits:

- Edit no more than **4 files per run**
- Do not perform global style/token refactors unless explicitly approved
- Only modify components directly involved in the selected **1–3 critical user flows** for this batch
- If issues are found outside those flows, add them to the backlog instead of fixing them now

### Behavior Guardrail

Do not change product interaction logic, navigation models, or feature behavior unless the current behavior creates an accessibility blocker.

If a behavior change is required for accessibility:

1. Explain why it is required
2. Keep the change minimal
3. Mark it clearly as a behavior change in the report

### Standards Guardrail

When proposing or implementing a fix:

- Prefer patterns aligned with **WCAG 2.1 / 2.2** and **ARIA Authoring Practices**
- When possible, cite the relevant WCAG Success Criterion
- Avoid citing WCAG criterion numbers unless you are confident they are correct

After completing a bounded batch, STOP and ask:

**Continue with next batch? (Y/N)**

If more issues remain, provide a short prioritized backlog and stop.

## Step 3 — Identify Critical User Flows (1–3)

List the **1–3 flows** that matter most.

Examples:

- login
- search / filter
- form submit
- checkout
- primary workflow
- admin dashboard action

Prioritize fixes inside these flows first.

If unclear, ask:

**Which 1–3 user actions are most important?**

## Step 4 — Enforce High-Weight Risk Categories

When identifying issues, classify them using the categories below and cite relevant WCAG Success Criteria when possible.

### A) Keyboard Operability (Release-blocker if broken)

Across critical flows:

- Everything interactive must be reachable and operable with keyboard
- Tab order must follow logical visual/reading order
- Visible focus must exist and be obvious
- No keyboard traps
- Custom interactions (menus, dialogs, sliders, tabs) must support correct keyboard behavior

### B) Focus Management (Common enterprise failure)

For any dynamic reveal (modal, drawer, popover, expanding region):

- When opened, focus must move into the revealed content
- While open, focus must not move behind it
- On close, focus must return to the triggering element
- Announcements must be appropriate but not excessive
- If unsure, prefer a standard dialog pattern

### C) Name / Role / Value (Semantics)

For every control:

- Use correct native element when possible
- Ensure accessible name (visible label or `aria-label`)
- Ensure role matches behavior
- Ensure state is exposed (expanded, pressed, selected, checked, disabled)

Avoid fake buttons or clickable `<div>` patterns.

### D) Forms & Errors

- Every input must have a programmatic label
- Required fields must not rely on color alone
- Error messages must be programmatically tied to fields
- Provide clear instructions

### E) Structure & Navigation

- Use landmarks (`header`, `nav`, `main`, `footer`) appropriately
- Only one primary `<main>` per page/view
- If layout repeats header/sidebar across pages, provide a **Skip to main content** link
- Maintain logical heading hierarchy

### F) Reflow / Resize

At 200% zoom and narrow width:

- Content must remain usable
- No critical controls cut off
- Dialogs must allow scrolling if content exceeds viewport

If changes require layout redesign, flag instead of forcing.

### G) Color & Contrast

Do not automatically change brand colors.

If contrast issues appear in critical flows:

- Flag the issue and explain the risk
- Cite the relevant WCAG Success Criterion when applicable (for example, WCAG 1.4.3 Contrast Minimum)

Only adjust styling when:

- The issue clearly violates accessibility guidance, and
- The change can be made locally without altering the global design system, and
- The issue materially affects a critical flow

Otherwise:

- Suggest safe alternatives
- Add the issue to the backlog

Do not change typography scales, design tokens, or color systems as a general remediation tactic unless explicitly approved.

### H) Non-Text Alternatives

- Decorative icons/images must be hidden from assistive tech
- Meaningful images must have appropriate text alternatives
- Charts must include text summary or data table alternative when feasible

### I) Complex Components (Enterprise emphasis)

If UI includes combobox, tabs, grid, tree, tooltip, dialog:

- Align with ARIA Authoring Practices (APG) patterns
- Prefer native controls if possible
- If implementing the full APG interaction model is non-trivial, do not introduce new ARIA widget roles (for example, `grid`, `tree`, `combobox`). Instead, prefer simpler native patterns (`button`, `list`, `input`) and flag the component as a risk requiring a full APG implementation.

## Step 5 — Mode Behavior

### BASIC-SAFETY

- Fix A–E where feasible
- Flag F–I if risky or large scope
- Keep explanations simple

### PUBLIC-FACING

- Fix A–H where feasible
- Flag complex component risks (I)
- Summarize remaining moderate risks

### ENTERPRISE-CRITICAL

- Fix A–I where feasible
- Classify remaining issues: **Critical / Serious / Cosmetic**
- Add procurement visibility: **High / Medium / Low**
- If Keyboard Operability or Focus Management fails in critical flows, do not mark as ready

## Final Output (Required)

Provide:

- Declared mode
- 1–3 critical user flows selected for this batch
- Issues identified (with category and severity)
- Changes made (grouped by category and file)
- WCAG references when applicable
- Remaining risks and prioritized backlog
- Suggested next actions
- Short defensibility note:
  - What is solid
  - What requires follow-up testing

End with this exact statement:

**This reduces risk; it does not guarantee full compliance.**

## Overlay-Symptom Guardrail

Never present this as a layer that “makes the UI accessible.”

This is code-level enforcement and risk reduction, and it must be re-runnable as the UI evolves.

## Example Report Output

```text
Declared mode: PUBLIC-FACING

Critical flows:
1. Signup form
2. Pricing CTA
3. Mobile navigation

Issues identified:
- Missing accessible names on icon buttons (C — Serious)
- Modal focus not returned to trigger (B — Critical)
- Signup form errors not tied to fields (D — Serious)

Changes made:
- Added aria-label to icon buttons in Navbar.tsx
- Added focus return logic in SignupModal.tsx
- Associated error messages with form inputs in SignupForm.tsx

Remaining risks:
- Contrast on muted helper text requires manual review
- Pricing comparison table may require a more complete semantic review

Suggested next actions:
- Run keyboard-only test on mobile nav and signup flow
- Verify modal behavior with screen reader

This reduces risk; it does not guarantee full compliance.
