# Skill: Introduce Design Patterns

## Purpose

Identify where applying a well-known design pattern would reduce coupling, improve extensibility, or clarify intent — and implement that pattern with discipline. Use this skill when code has recurring structural problems (conditional dispatch on type, repeated setup/teardown, scattered configuration) that a pattern would cleanly resolve.

## Instructions

You are acting as a senior software engineer introducing design patterns where appropriate. Follow these steps precisely:

1. **Identify the structural problem** — Name the specific code smell or structural issue before naming a pattern. Patterns are solutions to problems — the problem must be clearly articulated first. Common triggers: large switch/if-else chains that dispatch on type (→ Strategy/Visitor), repeated object construction code (→ Builder/Factory), direct dependencies on concrete classes making testing hard (→ Dependency Injection), cross-cutting concerns scattered across the codebase (→ Decorator/Middleware), resource lifecycle management (→ RAII/Template Method).

2. **Choose the right pattern** — Select the simplest pattern that addresses the problem. Do not choose a pattern because it is interesting — choose it because it fits. Document the alternative patterns considered and why they were rejected.

3. **Apply the pattern minimally** — Implement only as much of the pattern as the current use case requires. Do not build "flexibility" for hypothetical future cases. Prefer the simplest valid implementation: a Strategy with two implementations beats a factory that produces strategies via reflection.

4. **Maintain the existing interface** — Where possible, introduce the pattern behind the existing interface so callers don't change. Use the Strangler Fig approach: route through the new pattern while keeping the old entry point, then migrate callers incrementally.

5. **Document the pattern** — Add a comment naming the pattern, explaining why it was applied, and linking to the problem it solves. Teams vary in pattern knowledge — the comment ensures the intent survives the next refactor.

6. **Verify behavior is unchanged** — Run or describe tests that confirm the refactored code behaves identically to the original for all current use cases.

Constraints:
- Do not introduce a pattern just to use a pattern — every pattern adds abstraction layers that must be justified.
- Do not introduce Enterprise Java patterns (AbstractSingletonProxyFactoryBean) into languages and codebases that don't warrant that complexity.
- Do not introduce a pattern that requires widespread caller changes unless the benefit is substantial and the migration can be done atomically.
- If the pattern adds more code than it removes and the problem it solves is rare, reject it.

## Inputs

- **Source code**: The code exhibiting the structural problem.
- **Problem statement**: (Optional) The specific issue — "adding a new payment provider requires editing 6 files" — to anchor the pattern choice.
- **Constraints**: Language, framework, team familiarity with patterns.

## Output

- **Problem articulation**: The structural issue in plain English, with code location.
- **Pattern recommendation**: The chosen pattern, with rationale and rejected alternatives.
- **Implementation**: The refactored code applying the pattern, with all classes/interfaces/functions needed.
- **Interface changes**: What callers see before and after — ideally, no change.
- **Extensibility demonstration**: How the pattern makes the next change (e.g., adding a new strategy) simpler than before.
- **Test coverage**: Verification that behavior is unchanged and new implementations of the pattern can be tested in isolation.

---

> **Usage note**: Patterns are most valuable when introducing them before a known set of extensions. If extensions are hypothetical, use `reduce-complexity` or `extract-function-module` instead — simpler tools for simpler problems.
