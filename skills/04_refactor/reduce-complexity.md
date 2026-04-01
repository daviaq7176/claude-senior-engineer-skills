# Skill: Reduce Complexity

## Purpose

Simplify overly complex logic — reducing nesting, flattening conditionals, eliminating unnecessary abstraction layers, and making control flow easier to follow — without changing behavior. Use this skill when a function is hard to reason about, has high cyclomatic complexity, or requires excessive mental effort to understand.

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

You are acting as a senior software engineer simplifying complex logic. Follow these steps precisely:

1. **Measure current complexity** — Count the number of conditional branches (if/else/switch cases, ternaries, short-circuit evaluations, loop exit conditions) in the function. A function with more than 5–7 decision points is a candidate. Note the maximum nesting depth — more than 3 levels of indentation is a problem.

2. **Apply early returns (guard clauses)** — Find the outermost `if` blocks that wrap the main logic in success checks. Invert them: return early on the failure/error/skip condition and let the happy path run at the top level with no indentation. This eliminates one nesting level per guard.

3. **Flatten nested conditionals** — Find chains of `if/else if/else` that can be expressed as: a lookup table (map of values to outcomes), a switch/match statement, a polymorphic dispatch (strategy pattern), or a sequence of independent conditions with early returns. Choose the flattest representation.

4. **Eliminate boolean logic accumulation** — Find variables like `isValid = condition1 && condition2 && !condition3`. Extract named booleans or extract a function: `isEligibleForDiscount(user, order)`. The name makes the intent clear; the body documents the logic.

5. **Collapse redundant branches** — Find `if (x) return true; else return false` patterns and collapse to `return x`. Find `if (condition) { doA(); } else { doA(); doB(); }` patterns and merge the common parts.

6. **Break apart multi-concern functions** — If reducing complexity requires understanding that the function does two different things, use `extract-function-module` first to separate the concerns, then apply simplification to each piece independently.

Constraints:
- Do not change behavior — complexity reduction is a refactor, not a rewrite.
- Do not introduce complexity in the name of reducing it — a strategy pattern is not simpler than an if/else if the polymorphism adds more moving parts than it removes.
- Do not apply all patterns everywhere — match the technique to the specific complexity shape.
- Do not mix complexity reduction with other changes in the same commit.

## Inputs

- **Source code**: The function or module with high complexity.
- **Complexity metric**: (Optional) Specific metric or observation — cyclomatic complexity score, nesting depth, "impossible to understand" feedback.

## Output

- **Complexity analysis**: Current complexity assessment — nesting depth, branch count, cognitive load rating.
- **Techniques applied**: List of specific simplification techniques used and where.
- **Refactored code**: The simplified version, with the same observable behavior.
- **Before/after comparison**: Side-by-side or diff showing the change in structure.
- **Residual complexity**: Any complexity that couldn't be reduced, with explanation (e.g., inherent domain complexity).

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

> **Usage note**: Run `identify-risky-areas` first to confirm complexity is actually a risk. Pair with `rename-for-clarity` — clearer names often do as much as structural changes to reduce perceived complexity.
