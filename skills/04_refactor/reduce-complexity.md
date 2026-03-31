# Skill: Reduce Complexity

## Purpose

Simplify overly complex logic — reducing nesting, flattening conditionals, eliminating unnecessary abstraction layers, and making control flow easier to follow — without changing behavior. Use this skill when a function is hard to reason about, has high cyclomatic complexity, or requires excessive mental effort to understand.

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

---

> **Usage note**: Run `identify-risky-areas` first to confirm complexity is actually a risk. Pair with `rename-for-clarity` — clearer names often do as much as structural changes to reduce perceived complexity.
