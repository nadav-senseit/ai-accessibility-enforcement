# Evolution of AI Accessibility Enforcement Mode

(Retrospective — why the prompt is built the way it is)

This document explains how Accessibility Enforcement Mode evolved and why its guardrails exist.

If you only want to try the system, start here:

[Open prompt file](PROMPT.md)

---

# Why This Document Exists

Accessibility Enforcement Mode was not designed in a single step.

It evolved through repeated testing of AI coding tools (Cursor, Lovable, Claude Code) where we observed how models behave when asked to "fix accessibility."

This document records the reasoning behind the guardrails in the prompt.

It is not required to use the system.  
It exists for developers who want to understand *why the rules exist*.

---

# Version Timeline

v0.1 — Initial experiments with AI remediation  
v0.3 — Mode selection introduced  
v0.6 — Stability guardrails added  
v0.8 — Flow-based remediation model  
v1.0 — Contrast and complex component safety  
v1.1 — WCAG citation and defensibility language  
v1.1.4 — Session-level education discovery

---

# Phase 0 — The Original Problem

The initial problem was not *how to make AI fix accessibility.*

The real problem was:

AI coding agents tend to hallucinate accessibility fixes, over-edit codebases, and claim compliance they cannot guarantee.

In practice, we repeatedly saw agents:

• claiming “WCAG compliant”  
• inventing ARIA behaviors  
• rewriting large parts of the UI  
• breaking working code  
• drifting into design refactors instead of risk reduction

This made AI-generated accessibility dangerous in enterprise contexts, especially when:

• a VPAT is involved  
• a product is under procurement scrutiny  
• accessibility work must be defensible

So the first design principle emerged:

**Principle 1**

AI must reduce risk, not claim compliance.

This principle led directly to the first rule of the system.

---

# Phase 1 — Framing the Agent’s Role

The first versions focused on framing the agent correctly.

Instead of positioning it as an accessibility fixer, the prompt establishes:

This is NOT an accessibility overlay.  
This is NOT a guarantee of compliance.

The agent’s job became:

**Reduce high-risk accessibility failures in a defensible way.**

This framing solved three major issues:

• compliance hallucination  
• overconfidence in fixes  
• legal exposure in enterprise environments

This is why the prompt includes the instruction:

> If you cannot fix something reliably, flag it instead of guessing.

Without this constraint, LLMs often invent patterns.

---

# Phase 2 — Mode Selection (Risk Calibration)

Early testing revealed another issue.

Accessibility expectations differ significantly depending on the context.

| Context | Accessibility expectations |
|-------|---------------------------|
| Internal tool | minimal exposure |
| Public marketing site | moderate exposure |
| Enterprise SaaS | procurement scrutiny |

Without guidance, AI agents either:

• over-engineered everything  
or  
• ignored critical risks

So we introduced **Mode Selection**.
MODE: BASIC-SAFETY
MODE: PUBLIC-FACING
MODE: ENTERPRISE-CRITICAL


The system refuses to proceed without a declared mode.

This forces explicit intent and prevents the AI from guessing risk levels.

---

# Phase 3 — Preventing Tool Runaway

Once the agent started producing reasonable fixes, another problem appeared:

**Runaway editing.**

LLMs operating in code environments often:

• modify too many files  
• refactor design tokens  
• attempt system-wide fixes

This quickly breaks projects and exhausts tool limits.

To solve this we introduced **Stability Guardrails**:

• edit no more than 4 files per run  
• stop after each batch  
• ask the developer whether to continue

This turns the system into **controlled remediation** instead of uncontrolled editing.

The AI begins behaving more like:

a cautious engineer performing staged commits

instead of

an autonomous refactor engine.

---

# Phase 4 — Scope Guardrails

Accessibility agents frequently wander outside the actual user journey.

Example:

A fix intended for the signup flow turns into:

• typography refactors  
• icon changes  
• global color updates

This introduces risk and wastes engineering time.

So we added the **Scope Guardrail**:

Only modify components involved in the selected critical flows.

Everything else becomes backlog items.

This dramatically improved:

• precision  
• stability  
• developer trust

It also mirrors real accessibility audit methodology.

---

# Phase 5 — Flow-Based Accessibility

Accessibility problems are not evenly distributed.

They cluster around interaction-heavy flows such as:

• forms  
• dialogs  
• navigation  
• onboarding  
• checkout

So the prompt asks the AI to identify:

**1–3 critical user flows**

before making changes.

This ensures the AI focuses on areas that matter most for:

• usability  
• procurement evaluation  
• legal defensibility

---

# Phase 6 — Guarding Against Over-Fixing

AI agents strongly prefer fixing color contrast.

Even when:

• brand colors are intentional  
• the design system controls them  
• changing them would break brand identity

So we introduced the **Contrast Guardrail**:

Do not automatically change brand colors.  
Flag the issue instead.  
Suggest safe alternatives.

This protects design systems while still surfacing real accessibility risks.

---

# Phase 7 — Complex Component Safety

Modern interfaces contain advanced widgets such as:

• comboboxes  
• grids  
• tabs  
• dialogs

LLMs frequently invent ARIA patterns for these components.

Invented accessibility behavior is worse than broken accessibility.

So we added a strict rule:

Follow ARIA Authoring Practices patterns.

If the pattern is complex, flag the risk instead of inventing behavior.

---

# Phase 8 — WCAG Citation

Accessibility work must be defensible.

Enterprise teams expect issues to be linked to specific WCAG criteria.

So the prompt instructs the AI:

When possible, cite the relevant WCAG success criterion.

This transforms the output from:

AI suggestions

into:

audit-style documentation.

---

# Phase 9 — Defensibility Language

One line became central to the system:

> This reduces accessibility risk. It does not guarantee full compliance.

This protects:

• the user  
• the product team  
• the organization

because WCAG compliance cannot be guaranteed through automation alone.

---

# Phase 10 — The Session-Level Education Discovery

During testing we discovered something unexpected.

After running Accessibility Enforcement Mode, we asked the AI to build an unrelated component:

“Create a city filter dropdown.”

Accessibility was never mentioned.

Yet the generated component included:

• ARIA combobox attributes  
• keyboard navigation  
• accessible labeling  
• semantic structure

The model appeared to behave as if it had **internalized the enforcement rules.**

This suggests the prompt functions as a form of:

**session-level education.**

Once activated, accessibility becomes a default design constraint for the rest of the AI session.

---

Phase 11 — Field Experiment

After internal testing, the prompt was published publicly as an experiment to evaluate whether accessibility enforcement could operate inside real AI development workflows.

The goal of this phase is not reach or marketing.

The goal is learning.

Specifically:

• how different AI coding tools interpret the enforcement rules
• whether developers actually use the staged remediation model
• which accessibility issues models fix reliably
• where models hallucinate or introduce risk

Feedback from this experiment will inform the next generation of accessibility infrastructure inside development workflows.

---

# Why This Matters

If this behavior proves consistent across tools, enforcement prompts could serve two roles:

1. **Remediation**

Fix accessibility issues in existing code.

2. **Preventive guidance**

Influence how AI generates new UI during the session.

Accessibility could move from:

post-release audits

to

a development-time constraint.

---

# What the System Ultimately Became

After multiple iterations the prompt functions as:

**A controlled accessibility remediation protocol.**

It enforces:

• risk-based prioritization  
• controlled code changes  
• audit-style documentation  
• compliance humility

The AI behaves less like:

a generative model

and more like:

a cautious accessibility engineer performing staged remediation.
