# Skill: Patch Critical Bug

## Purpose

Apply a minimal, safe, targeted fix to a confirmed bug — changing exactly what needs to change and nothing more. Use this skill when the root cause is understood, the fix is urgent or high-stakes, and correctness and minimal blast radius are the top priorities.

## Instructions

You are acting as a senior software engineer applying a surgical bug fix. Follow these steps precisely:

1. **Confirm the root cause** — Before writing any code, state the root cause in one sentence. If it cannot be stated clearly, stop and use `trace-execution-path` first. A fix applied to the wrong cause will not work and may make things worse.

2. **Scope the fix** — Define the minimum change needed to address the root cause. Ask: can this be fixed in one function? One file? Does it require a data migration? Does it require a schema change? Each additional scope element adds risk.

3. **Write the fix** — Implement the change. Prefer: correcting the logic over restructuring the code, using existing abstractions over introducing new ones, and adding a guard clause over reorganizing a function.

4. **Verify behavior is preserved** — Check that every other code path through the affected function still behaves correctly. The fix must not break the non-broken cases. Trace through callers to verify the interface contract is unchanged.

5. **Add a regression guard** — Write or describe a test that would have caught this bug before it reached production. If a test cannot be written quickly, at minimum add a log statement that would have made the bug visible in the next occurrence.

6. **Document the fix** — Write a clear commit message or code comment explaining: what the bug was, why this fix works, and any assumptions the fix depends on.

Constraints:
- Do not refactor while fixing — separate concerns, separate commits.
- Do not change function signatures, return types, or public interfaces unless the bug is in the contract itself.
- Do not add configuration flags or feature toggles unless rollback risk is extremely high — they become permanent.
- Do not fix what isn't broken — every additional line is a risk vector.
- If the correct fix requires significant restructuring, apply the minimal patch now and open a follow-up task for the proper refactor.

## Inputs

- **Source code**: The buggy function(s) and their immediate callers.
- **Root cause**: A clear statement of what is wrong and why.
- **Reproduction case**: The input or scenario that triggers the bug.

## Output

- **Fix description**: One paragraph explaining what changed and why it fixes the bug.
- **Code diff**: The exact change — shown as a diff or annotated before/after snippet.
- **Impact assessment**: What other code is affected by this change, and confirmation it is not broken.
- **Regression test**: A test case (or description of one) that validates the fix and would catch a regression.
- **Commit message**: A ready-to-use commit message following conventional commit format.
- **Follow-up items**: Any technical debt or refactor work this fix defers, noted for the backlog.

---

> **Usage note**: Never skip step 1. A fix without a confirmed root cause is a guess. Use `automated-test-generation` to harden the regression test, and `security-review` if the bug has any security implications.
