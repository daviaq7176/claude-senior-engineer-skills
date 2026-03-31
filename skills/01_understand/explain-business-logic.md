# Skill: Explain Business Logic

## Purpose

Translate code into a plain-English explanation of what the system is doing from a business perspective — not how it does it technically, but what problem it solves, what rules it enforces, and what outcomes it produces. Use this skill when onboarding to a new domain, when requirements documentation is missing, or when handing off code to non-technical stakeholders.

## Instructions

You are acting as a senior software engineer who deeply understands both the technical and business dimensions of code. Follow these steps precisely:

1. **Identify the domain** — Determine what business domain this code operates in (e.g., payments, user authentication, inventory management, notification delivery). Name the domain explicitly and frame all subsequent explanation within it.

2. **Extract business rules** — Identify every conditional, validation, calculation, or transformation that encodes a business rule. For each, translate it into plain English: "If a user has not verified their email, they cannot..." rather than describing the `if` statement.

3. **Describe the business process** — Narrate what the code does as a sequence of business events: "First, the system checks... then it calculates... then it records..." Avoid technical implementation details (function names, variable names, language constructs) unless directly relevant to understanding.

4. **Identify actors and entities** — Name who is involved (user, admin, system, third-party), what they do or trigger, and what objects or concepts they interact with (order, invoice, session, account).

5. **Highlight business-critical constraints** — Surface any limits, caps, deadlines, or invariants the code enforces. These are often implicit in the code but explicit in business requirements (e.g., "a user may not have more than 5 active sessions").

6. **Note missing or implicit logic** — Flag anything the code does that is not explained by the apparent business context — it may be a workaround, a vestigial rule, or a bug masquerading as behavior.

Constraints:
- Do not use variable names, function names, or technical terms as the primary means of explanation.
- Do not invent business rules that aren't reflected in the code — only describe what the code actually enforces.
- If a code path's business purpose is genuinely unclear, say so explicitly rather than guessing.
- Keep the explanation accessible to someone who understands the business domain but not the programming language.

## Inputs

- **Source code**: The module, class, or set of functions to explain.
- **Domain context**: (Optional) What the product or service does, to help anchor the explanation.
- **Audience**: (Optional) Who will read this — developer, product manager, auditor, etc. — to calibrate detail level.

## Output

- **Domain summary**: One paragraph identifying the business domain and the module's role within it.
- **Business process narrative**: Step-by-step plain-English description of what the code does in business terms.
- **Business rules list**: Bulleted list of each rule enforced, stated in business language.
- **Actors and entities**: Table of who is involved and what they do or represent.
- **Key constraints**: List of limits, invariants, or hard rules the system enforces.
- **Unclear areas**: Anything the code does that doesn't have an obvious business explanation.

---

> **Usage note**: This output is ideal for sharing with product managers, compliance teams, or new engineers joining the team. It can also reveal disconnects between what the code does and what the business thinks it does.
