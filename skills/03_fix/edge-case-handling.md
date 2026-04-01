# Skill: Edge Case Handling

## Purpose

Identify and add handling for edge cases and boundary conditions that the current code does not account for. Use this skill after writing or reviewing a function to harden it against unexpected inputs, extreme values, and real-world conditions the happy path doesn't cover.

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

You are acting as a senior software engineer systematically hardening code against edge cases. Follow these steps precisely:

1. **Enumerate the input space** — For each input parameter, list the range of possible values including: null/undefined/None, empty string/array/object, zero and negative numbers, very large numbers, maximum allowed values, whitespace-only strings, special characters, non-ASCII characters, and values of the wrong type.

2. **Check boundary conditions** — For any operation involving ranges, indices, or thresholds, verify behavior at: the exact boundary, one below the boundary, one above the boundary, and the maximum representable value. Off-by-one errors live here.

3. **Identify missing state checks** — Find calls to methods or operations that assume a certain state (object is initialized, list is non-empty, connection is open, user is authenticated). For each, verify there is a guard before the assumption is relied on.

4. **Test the empty/zero case** — Verify the code handles: empty collections in loops, zero divisors in divisions, empty results from database queries, zero-length files, and empty responses from external APIs.

5. **Consider concurrent and repeated invocations** — What happens if the function is called twice with the same input? What if it's called before a prior call completes? What if it's called while the system is shutting down?

6. **Write the missing cases** — For each unhandled edge case found, add: an explicit guard or validation, a meaningful error or return value, and a comment explaining the edge case if it is non-obvious.

Constraints:
- Do not add defensive checks that are already guaranteed by the caller — over-defensive code becomes noise.
- Do not silently swallow errors — every edge case that is caught must either be corrected, returned, or re-raised with context.
- Do not change the happy-path behavior while adding edge case handling.
- Prefer early returns over nested conditionals for guard clauses.

## Inputs

- **Source code**: The function or module to harden.
- **Known failure cases**: (Optional) Specific edge cases that have already caused problems.
- **Input contracts**: (Optional) What callers are documented or expected to pass.

## Output

- **Edge case inventory**: Comprehensive list of edge cases found, categorized by type (null, boundary, state, concurrent).
- **Current behavior**: What the code does today for each unhandled case (crash, wrong result, silent pass-through).
- **Code additions**: The specific guard clauses, validations, and error returns to add — shown as diffs or annotated snippets.
- **Test cases**: One test per edge case, covering the input and expected behavior after the fix.
- **Remaining risks**: Edge cases that were identified but not handled, with rationale for deferring.

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

> **Usage note**: Run this skill after `patch-critical-bug` to harden beyond the immediate fix, and before `automated-test-generation` to ensure the tests cover all the important cases.
