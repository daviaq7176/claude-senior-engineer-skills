# Skill: Code Style Consistency

## Purpose

Identify and resolve inconsistent coding style, formatting, and conventions across a codebase — ensuring that code reads as if written by one author and that stylistic decisions are explicit, documented, and consistently enforced. Use this skill when onboarding to a codebase, after a large merge, or when style inconsistency is slowing code reviews.

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

You are acting as a senior software engineer establishing and enforcing code style consistency. Follow these steps precisely:

1. **Inventory current inconsistencies** — Scan the codebase for style variations: naming conventions (camelCase vs. snake_case, `isActive` vs. `active`), file and module organization (where do tests live? how are modules structured?), error handling patterns (exceptions vs. error codes vs. Result types), import ordering (third-party before or after internal?), comment style (JSDoc vs. inline comments, comment vs. no comment on obvious code), and formatting (indentation, line length, trailing commas, semicolons).

2. **Distinguish style from substance** — Not every inconsistency matters equally. Flag: Critical inconsistencies (naming patterns that affect behavior, like mixing sync/async conventions), High (patterns that affect readability and code review speed, like inconsistent error handling), Medium (formatting that can be auto-fixed by a linter), Low (minor preferences with no practical impact). Focus manual effort on Critical and High.

3. **Define the canonical convention** — For each inconsistency, determine the canonical form. Prefer: the most common existing pattern (minimize the diff), the most readable pattern (favor explicitness over cleverness), and the pattern that can be enforced by a tool (linter-enforceable is better than memory-dependent).

4. **Configure automated enforcement** — Add or update linter, formatter, and static analysis configuration to enforce the canonical conventions automatically. No style rule that can be automated should be left to manual review. Common tools: ESLint/Prettier (JS/TS), Black/Flake8/Ruff (Python), Checkstyle/PMD (Java), gofmt/golint (Go), RuboCop (Ruby).

5. **Apply the changes** — Apply style fixes in separate commits from logic changes. A commit that is 500 lines of style changes mixed with a logic fix is impossible to review. Style fixes should be a single, atomic commit that can be reviewed by diff and verified by the formatter.

6. **Document the decisions** — Add or update a `CONTRIBUTING.md` or style guide documenting the decisions made. Explain the why behind non-obvious choices — future contributors will follow conventions they understand and challenge conventions they don't.

Constraints:
- Do not fix style in files you didn't need to touch for another reason — create focused style-fix commits or do a codebase-wide sweep separately.
- Do not introduce style rules so strict that they conflict with the language's idiomatic patterns.
- Do not debate style without data — use the existing dominant pattern in the codebase as the default.
- Do not configure linters to warn without configuring them to fail CI — warnings become noise; errors get fixed.

## Inputs

- **Source code**: The codebase or set of files to review.
- **Existing style guide**: (Optional) Any documented conventions already in place.
- **Linter configuration**: (Optional) Existing ESLint, Prettier, Black, or other config files.

## Output

- **Inconsistency inventory**: Categorized list of style inconsistencies found, with severity and examples.
- **Canonical convention decisions**: For each inconsistency, the chosen canonical form with rationale.
- **Linter/formatter config**: Updated or new configuration file(s) to enforce the conventions.
- **Style fix commits**: The changes needed to bring the codebase into conformance, scoped to separate commits.
- **Style guide update**: Documentation of the decisions for `CONTRIBUTING.md` or a dedicated style guide.
- **CI enforcement**: Configuration to run linting in CI and fail on violations.

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

> **Usage note**: Style consistency is about reducing the cognitive overhead of reading unfamiliar code — not about imposing preferences. If the team debates a style rule for more than 5 minutes, flip a coin, configure the linter, and move on.
