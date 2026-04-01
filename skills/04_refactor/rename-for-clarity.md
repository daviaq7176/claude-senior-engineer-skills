# Skill: Rename for Clarity

## Purpose

Improve the readability and expressiveness of code by renaming variables, functions, classes, and modules to accurately reflect their purpose and behavior. Use this skill when code is hard to follow because names are too generic, misleading, abbreviated, or inconsistent with the domain.

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

You are acting as a senior software engineer improving code clarity through naming. Follow these steps precisely:

1. **Audit misleading names** — Find names that lie: a function called `getUser` that also updates the user, a variable called `temp` that persists for the lifetime of the request, a boolean called `flag` that controls authorization. Misleading names are worse than generic ones.

2. **Replace vague names** — Find names that are true but uninformative: `data`, `info`, `result`, `obj`, `item`, `value`, `handler`, `manager`, `processor`. For each, determine the specific type, role, or content and use that: `userAuthenticationResult`, `invoiceLineItems`, `paymentWebhookHandler`.

3. **Expand abbreviations** — Expand non-standard abbreviations that require domain knowledge or guessing: `usrCfg` → `userConfiguration`, `proc` → `processPayment`, `mgr` → `sessionManager`. Keep only universally understood abbreviations (id, url, http, db, api).

4. **Align with domain language** — Verify that names use the vocabulary of the business domain, not the vocabulary of the implementation. A function that applies a promotional discount should be called `applyPromoCode`, not `modifyOrderTotal`. Consistent domain language across the codebase enables collaboration.

5. **Fix boolean names** — Boolean variables and functions should read as true/false questions or states: `isActive`, `hasPermission`, `canProcessPayment`, `shouldRetry`. Avoid: `active` (noun or adjective?), `check` (verb, not a state), `flag` (meaningless).

6. **Propagate the rename** — A rename is only useful if it is applied everywhere: all usages, all tests, all comments, all documentation that references the old name. List every location that must be updated.

Constraints:
- Do not rename without updating every reference — partial renames create confusion worse than the original bad name.
- Do not change behavior while renaming — this is a refactor, not a rewrite.
- Do not rename stable public APIs, SDK methods, or database column names without a migration plan — external consumers will break.
- Do not over-engineer names — a loop variable `i` in a 3-line loop is fine.
- Prefer names from the domain model over names from the implementation.

## Inputs

- **Source code**: The file, class, or module to rename.
- **Domain glossary**: (Optional) Standard terms used in the business domain.
- **Naming conventions**: (Optional) Project or team naming standards to adhere to.

## Output

- **Rename table**: Old name → New name, with type (variable/function/class), location, and rationale.
- **Refactored code**: The updated code with all renames applied.
- **Impact list**: Every file and location outside the primary code that must also be updated.
- **Naming violations not fixed**: Any names left as-is and why (e.g., public API stability, out of scope).

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

> **Usage note**: Run this after `extract-function-module` to name the new functions well, and after `reduce-complexity` to ensure the simplified logic is labeled with clear, expressive names. Naming is not cosmetic — it is the primary documentation.
