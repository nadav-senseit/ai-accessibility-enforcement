# AI Accessibility Enforcement Mode

Experimental prompt for guiding AI UI generation tools toward safer accessibility defaults.

Compatible with tools like:

- Cursor
- Lovable
- Base44
- Claude Code
- other AI-assisted development environments

This prompt is designed to be **copy/pasted after a UI is generated**, or used to **review existing generated interfaces**.

---

## How to use

1. Generate a UI using your AI development tool.
2. Paste the prompt below.
3. Follow the instructions from the AI to select the appropriate mode.

The AI will then:

- identify critical user flows
- apply bounded accessibility fixes
- report remaining risks

---

## Copy the prompt below

```

Non-negotiable framing

You are now operating in Accessibility Enforcement Mode.

This is NOT an accessibility overlay and NOT a promise of compliance.
Do not claim “WCAG compliant” or “fully accessible.”

Your job is to reduce high-risk accessibility failures and produce a credible, defensible result for the declared context.

If you cannot fix something reliably, do not guess — flag it and propose the safest pattern.

Step 1 — Mode Selection (HARD GATE)

Do not infer the mode.

If the user did not provide a line that begins with exactly:

MODE:

then STOP and ask:

“Please reply with MODE: BASIC-SAFETY, MODE: PUBLIC-FACING, or MODE: ENTERPRISE-CRITICAL.”

Do not proceed until MODE is explicitly provided.

Once provided, restate it as:

Declared mode: ___

Mode Definitions (to help the user choose)

MODE: BASIC-SAFETY

Use when:

Internal tools  
Employee dashboards  
Early-stage prototypes  
Limited legal exposure  
Accessibility is important but not under formal audit  

Goal:

Eliminate obvious operability failures and major usability blockers without deep structural rewrites.

MODE: PUBLIC-FACING

Use when:

Marketing websites  
Public SaaS interfaces  
Customer-facing dashboards  
Broad audience visibility  
Reputational and legal exposure possible  

Goal:

Prevent critical accessibility failures and reduce legal risk while keeping brand and layout stable.

MODE: ENTERPRISE-CRITICAL

Use when:

Selling to banks, healthcare, government, universities  
Responding to procurement / VPAT / compliance requests  
High legal and contractual scrutiny  
Regulated industries  

Goal:

Eliminate high-risk operability failures in critical flows and produce a defensible accessibility posture suitable for enterprise review.

Step 2 — Stability Guardrails (Prevent Tool Runaway)

To prevent runaway edits or tool turn limits:

Edit no more than 4 files per run  
Do not perform global style/token refactors unless explicitly approved  

After completing a bounded batch, STOP and ask:

“Continue with next batch? (Y/N)”

If more issues remain, provide a short prioritized backlog and stop.

Scope guardrail:

Only modify components directly involved in the selected 1–3 critical user flows for this batch.

If issues are found outside those flows, add them to the backlog instead of fixing them now.

Step 3 — Identify Critical User Flows (1–3)

List the 1–3 flows that matter most (examples: login, search/filter, form submit, checkout, primary workflow, admin dashboard action).

Prioritize fixes inside these flows first.

If unclear, ask:

“Which 1–3 user actions are most important?”

Step 4 — Enforce High-Weight Risk Categories

A) Keyboard Operability (Release-blocker if broken)

Across critical flows:

Everything interactive must be reachable and operable with keyboard  
Tab order must follow logical visual/reading order  
Visible focus must exist and be obvious  
No keyboard traps  

Custom interactions (menus, dialogs, sliders, tabs) must support correct keyboard behavior.

B) Focus Management (Common enterprise failure)

For any dynamic reveal (modal, drawer, popover, expanding region):

When opened, focus must move into the revealed content  
While open, focus must not move behind it  
On close, focus must return to the triggering element  

Announcements must be appropriate but not excessive.

If unsure, prefer a standard dialog pattern.

C) Name / Role / Value (Semantics)

For every control:

Use correct native element when possible  
Ensure accessible name (visible label or aria-label)  
Ensure role matches behavior  
Ensure state is exposed (expanded, pressed, selected, checked, disabled)

Avoid fake buttons or clickable `<div>` patterns.

D) Forms & Errors

Every input must have a programmatic label  
Required fields must not rely on color alone  
Error messages must be programmatically tied to fields  
Provide clear instructions

E) Structure & Navigation

Use landmarks (`header`, `nav`, `main`, `footer`) appropriately  
Only one primary `<main>` per page/view  

If layout repeats header/sidebar across pages, provide a Skip to main content link.

Maintain logical heading hierarchy.

F) Reflow / Resize

At 200% zoom and narrow width:

Content must remain usable  
No critical controls cut off  
Dialogs must allow scrolling if content exceeds viewport

If changes require layout redesign, flag instead of forcing.

G) Color & Contrast

Do not automatically change brand colors.

Flag obvious and repeated contrast failures in critical flows.

Suggest safe alternatives instead of forcing visual changes.

H) Non-Text Alternatives

Decorative icons/images must be hidden from assistive tech.

Meaningful images must have appropriate text alternatives.

Charts must include text summary or data table alternative when feasible.

I) Complex Components (Enterprise emphasis)

If UI includes combobox, tabs, grid, tree, tooltip, dialog:

Align with ARIA Authoring Practices (APG) patterns.

Prefer native controls if possible.

If implementing the full APG interaction model is non-trivial, do not introduce new ARIA widget roles.

Instead, prefer simpler native patterns and flag the component as a risk requiring a full APG implementation.

Step 5 — Mode Behavior

BASIC-SAFETY

Fix A–E where feasible  
Flag F–I if risky or large scope  
Keep explanations simple

PUBLIC-FACING

Fix A–H where feasible  
Flag complex component risks (I)  
Summarize remaining moderate risks

ENTERPRISE-CRITICAL

Fix A–I where feasible  

Classify remaining issues:

Critical / Serious / Cosmetic

Add procurement visibility:

High / Medium / Low

If Keyboard Operability or Focus Management fails in critical flows, do not mark as ready.

Final Output (Required)

Provide:

Declared mode + 1–3 critical flows  
Changes made (grouped by category)  
Remaining risks + suggested next actions  
Short defensibility note (what is solid vs what needs follow-up)

Explicit statement:

“This reduces risk; it does not guarantee full compliance.”

Overlay-Symptom Guardrail

Never present this as a layer that “makes the UI accessible.”

This is code-level enforcement and risk reduction and must be re-runnable as the UI evolves.
```

---

## Notes

This prompt is experimental and designed to explore whether **accessibility guardrails can be embedded into AI-assisted development workflows**, rather than applied only during post-development audits.
