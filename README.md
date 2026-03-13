# ai-accessibility-enforcement

Experiment: enforcing accessibility guardrails in AI-generated interfaces (Cursor, Lovable, Base44, Claude Code) using prompt-level instructions.

⚠️ Early experiment. Real-world tests and feedback are extremely valuable.

---

## What this experiment does

AI development tools can now generate entire interfaces.

But accessibility is rarely enforced during generation, and the result is that many AI-generated UIs ship with basic accessibility failures.

This experiment explores whether a simple **prompt-level enforcement loop** can help reduce common accessibility problems **during development**, rather than fixing them later.

The enforcement prompt instructs the AI to:

- Identify critical user flows in the interface
- Check a small set of high-impact accessibility requirements
- Apply bounded fixes where appropriate
- Produce a short report of what was improved and what remains risky

The goal is **not compliance**.

The goal is **reducing accessibility risk during AI-assisted development**.

---

## How the enforcement loop works

When enforcement mode runs, the AI follows a simple loop:

1. Identify critical flows (e.g., login, checkout, settings modal)
2. Evaluate key accessibility areas:
   - keyboard access
   - focus behavior
   - labels
   - semantic structure
3. Apply bounded fixes
4. Produce a short report:
   - what changed
   - what remains risky

---

## Example

### Without enforcement

~~~html
<button><svg></svg></button>
~~~

Problems:

- No accessible name
- Screen reader users cannot understand the button
- The purpose of the control is unclear

### With enforcement mode

~~~html
<button aria-label="Open settings">
  <svg aria-hidden="true"></svg>
</button>
~~~

Fix applied:

- Accessible label added
- Decorative SVG hidden from screen readers
- Control becomes understandable to assistive technologies
- No visual change required

## Example: modal focus management

AI-generated modals often miss keyboard support.

### Before

~~~html
<div class="modal">
  <button>X</button>
  <input placeholder="Email">
</div>
~~~

Problems:

- No dialog role
- No focus management expectations
- Screen reader context unclear

### With enforcement mode

~~~html
<div role="dialog" aria-modal="true" aria-labelledby="modal-title">
  <h2 id="modal-title">Subscribe</h2>

  <button aria-label="Close dialog">×</button>

  <label for="email">Email</label>
  <input id="email" type="email">
</div>
~~~

Fix applied:

- Dialog semantics added
- Accessible name provided
- Form label added
- Focus behavior becomes predictable for assistive technology

---

## What this experiment is **not**

This experiment does **not** claim to:

- Automatically produce WCAG compliance
- Replace accessibility testing
- Replace accessibility expertise
- Fix every accessibility issue

Accessibility remains grounded in structured markup, testing, and real user validation.

This experiment only explores whether **AI generation can be guided toward safer defaults.**

---

## Try it in 30 seconds

1. Copy the prompt from the file below
2. Paste it into your AI development tool (Cursor, Lovable, Base44, Claude Code, etc.)
3. Ask the AI to review or generate a UI using the enforcement instructions

Prompt file:

[Open the prompt](prompt/enforcement-mode.md)

Use it when:

- generating new interfaces
- reviewing generated code
- asking the AI to improve an existing UI

---

## What the prompt focuses on

The enforcement instructions focus on a small set of high-impact areas:

- Keyboard operability
- Focus management
- Accessible labeling
- Semantic HTML structure
- Avoiding unnecessary ARIA
- Bounded changes + short report (so developers can review diffs)

These issues represent a large portion of real accessibility failures in generated interfaces.

---

## Example output report

Example of a short enforcement summary the AI might produce:

~~~
Accessibility Enforcement Summary

Critical flows identified:
- Login form
- Settings modal

Fixes applied:
✔ Added labels to icon-only buttons
✔ Corrected focus order in modal
✔ Replaced ARIA button with native button

Remaining risks:
- Contrast/reflow may require manual review (brand/layout sensitive)
- Screen reader testing recommended
- Complex components may require APG-aligned implementation
~~~

---

## Why this matters

AI is increasingly used to generate interfaces.

If accessibility is not considered during generation, accessibility problems will scale with it.

This experiment explores whether accessibility guidance can become part of **the development infrastructure itself**, rather than an afterthought.

---

## Why the prompt looks like this

Accessibility Enforcement Mode did not appear fully formed.

It evolved through multiple iterations while testing AI coding tools such as Cursor, Lovable, and Claude Code.

During testing we repeatedly observed several failure modes:

• accessibility hallucinations  
• uncontrolled code refactors  
• invented ARIA behaviors  
• false WCAG compliance claims  
• accessibility fixes drifting into UI redesign

Each rule in the prompt was introduced to address one of these patterns.

Over time the system evolved into a **controlled remediation protocol** designed to reduce accessibility risk during AI-assisted development.

Instead of behaving like a generative assistant that freely rewrites code, the AI is guided to behave more like a cautious accessibility engineer performing staged remediation.

If you're interested in the reasoning behind each guardrail and how the system evolved:

→ See [**EVOLUTION.md**](EVOLUTION.md) for the full retrospective.

---
## Feedback welcome

If you try this experiment, feedback is extremely valuable.

Useful feedback includes:

- Which AI tool you used
- What the prompt fixed well
- What it broke
- Whether something like this would be useful in your workflow

Feel free to open an issue or discussion.

---

## Version History

### v1.1.4 (Current)

Improvements to guardrails, reporting structure, and standards alignment.

Changes:

• Added guidance to reference WCAG Success Criteria when identifying issues  
• Improved stability guardrails to reduce runaway edits in AI tools  
• Clarified contrast handling to avoid unintended design system changes  
• Expanded output requirements (issues list, WCAG references, prioritized backlog)  
• Improved complex component guidance to avoid unsafe ARIA role introduction  
• Strengthened batch-edit scope rules tied to critical user flows  

This version improves the prompt’s ability to produce **traceable, defensible accessibility fixes** while remaining safe for AI-assisted development environments.

---

### v1.0

Initial experimental release.

Focus:

• Hard-gated enforcement modes  
• Critical user flow prioritization  
• High-weight accessibility risk categories  
• Guardrails to prevent unsafe automated changes

---

Maintained by the team at **Senseit**, exploring infrastructure approaches to accessibility in AI-generated interfaces.

