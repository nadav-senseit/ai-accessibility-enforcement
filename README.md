# ai-accessibility-enforcement

Experiment: enforcing accessibility guardrails in AI-generated interfaces (Cursor, Lovable, Base44, Claude Code) using prompt-level instructions.

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
  <svg></svg>
</button>
~~~

Fix applied:

- Accessible label added
- Control becomes understandable to assistive technologies
- No visual change required

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

~~~
prompt/enforcement-mode.md
~~~

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
- Color contrast not evaluated
- Screen reader testing recommended
~~~

---

## Why this matters

AI is increasingly used to generate interfaces.

If accessibility is not considered during generation, accessibility problems will scale with it.

This experiment explores whether accessibility guidance can become part of **the development infrastructure itself**, rather than an afterthought.

---

## Feedback welcome

If you try this experiment, feedback is extremely valuable.

Useful feedback includes:

- Which AI tool you used
- What the prompt fixed well
- What it broke
- Whether something like this would be useful in your workflow

Feel free to open an issue or discussion.
