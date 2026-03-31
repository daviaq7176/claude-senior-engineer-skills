# Skill: Code Style Consistency

## Purpose

Identify and resolve inconsistent coding style, formatting, and conventions across a codebase — ensuring that code reads as if written by one author and that stylistic decisions are explicit, documented, and consistently enforced. Use this skill when onboarding to a codebase, after a large merge, or when style inconsistency is slowing code reviews.

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

---

> **Usage note**: Style consistency is about reducing the cognitive overhead of reading unfamiliar code — not about imposing preferences. If the team debates a style rule for more than 5 minutes, flip a coin, configure the linter, and move on.
