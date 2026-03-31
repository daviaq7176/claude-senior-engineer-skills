# Skill: Rename for Clarity

## Purpose

Improve the readability and expressiveness of code by renaming variables, functions, classes, and modules to accurately reflect their purpose and behavior. Use this skill when code is hard to follow because names are too generic, misleading, abbreviated, or inconsistent with the domain.

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

---

> **Usage note**: Run this after `extract-function-module` to name the new functions well, and after `reduce-complexity` to ensure the simplified logic is labeled with clear, expressive names. Naming is not cosmetic — it is the primary documentation.
