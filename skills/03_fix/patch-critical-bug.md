# Skill: Patch Critical Bug

## Purpose

Apply a minimal, safe, targeted fix to a confirmed bug — changing exactly what needs to change and nothing more. Use this skill when the root cause is understood, the fix is urgent or high-stakes, and correctness and minimal blast radius are the top priorities.

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

> **Usage note**: Never skip step 1. A fix without a confirmed root cause is a guess. Use `automated-test-generation` to harden the regression test, and `security-review` if the bug has any security implications.
