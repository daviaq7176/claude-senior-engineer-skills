# Skill: Introduce Design Patterns

## Purpose

Identify where applying a well-known design pattern would reduce coupling, improve extensibility, or clarify intent — and implement that pattern with discipline. Use this skill when code has recurring structural problems (conditional dispatch on type, repeated setup/teardown, scattered configuration) that a pattern would cleanly resolve.

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

> **Usage note**: Patterns are most valuable when introducing them before a known set of extensions. If extensions are hypothetical, use `reduce-complexity` or `extract-function-module` instead — simpler tools for simpler problems.
