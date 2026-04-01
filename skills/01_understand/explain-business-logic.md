# Skill: Explain Business Logic

## Purpose

Translate code into a plain-English explanation of what the system is doing from a business perspective — not how it does it technically, but what problem it solves, what rules it enforces, and what outcomes it produces. Use this skill when onboarding to a new domain, when requirements documentation is missing, or when handing off code to non-technical stakeholders.

## Reasoning Protocol

Before producing any output, work through these internal reasoning steps and show your work:

<reasoning>
1. **Observation** — What do I literally see in the code/input? List concrete facts without interpretation.
2. **Pattern Recognition** — What patterns, idioms, anti-patterns, or known problem types does this match?
3. **Hypothesis Formation** — What are the 2-3 most likely explanations or approaches? Briefly state each.
4. **Evidence Gathering** — For each hypothesis, what specific evidence (file:line, behavior, logs) supports or refutes it?
5. **Conclusion** — Given the evidence, what is the most likely answer? Why were alternatives ruled out?
</reasoning>

This structured thinking must appear in your response before conclusions. Do not skip to answers.

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

## Self-Verification Checklist

Before finalizing output, verify each item:

- [ ] **Evidence grounded**: Every claim cites a specific file:line, code snippet, or observable behavior
- [ ] **No speculation as fact**: Uncertainties are clearly marked, not presented as conclusions
- [ ] **Edge cases considered**: Obvious edge cases and failure modes have been addressed
- [ ] **Assumptions explicit**: All assumptions about context, environment, or intent are stated
- [ ] **Counter-arguments addressed**: The strongest objection to your analysis has been considered
- [ ] **Scope maintained**: The response addresses what was asked — no scope creep, no omissions

## Confidence Assessment

Tag your overall findings and each major conclusion:

| Level | Criteria | Action Required |
|-------|----------|-----------------|
| 🟢 **High** (90%+) | Direct evidence in code, deterministic behavior, well-understood pattern | Proceed with recommendation |
| 🟡 **Medium** (60-90%) | Strong indicators but depends on runtime, config, or external state | Note assumptions, suggest verification |
| 🔴 **Low** (<60%) | Inference from incomplete information, multiple plausible interpretations | Require additional information before acting |

State your confidence level and the key factor that determines it.

## Adversarial Self-Review

After completing your analysis, challenge yourself:

1. **What did I possibly miss?** — Blind spots, unstated assumptions, paths not explored
2. **What would a skeptical senior engineer challenge?** — The weakest link in your reasoning
3. **What's an alternative interpretation?** — Could the same evidence support a different conclusion?

Address the most significant challenge in your response. If you cannot adequately address it, flag it as a known limitation.

---

> **Usage note**: This output is ideal for sharing with product managers, compliance teams, or new engineers joining the team. It can also reveal disconnects between what the code does and what the business thinks it does.
