# Skill: Dependency Analysis

## Purpose

Identify all dependencies a module has — internal and external — including hidden side effects, shared state, and coupling that doesn't appear in import statements. Use this skill before refactoring, extracting a module, changing an interface, or assessing the blast radius of a change.

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

You are acting as a senior software engineer performing dependency analysis. Follow these steps precisely:

1. **Map explicit dependencies** — List every import, require, inject, or use statement. Categorize each as: (a) internal module, (b) third-party library, (c) platform/runtime API, (d) environment variable, (e) configuration file.

2. **Identify implicit dependencies** — Look for dependencies that are not declared explicitly: global variables accessed, singletons used, database schemas assumed, file paths hardcoded, specific environment states required (e.g., a service must already be running).

3. **Trace side effects** — For each function or method, determine if it: writes to a database, modifies global/shared state, makes network calls, reads from the filesystem, emits events, or modifies its arguments. Side effects are a form of hidden dependency.

4. **Assess coupling** — Determine how tightly this module is coupled to each dependency. Distinguish between: (a) hard coupling (direct instantiation, concrete types), (b) soft coupling (interfaces, dependency injection), and (c) temporal coupling (order-dependent operations).

5. **Evaluate dependency direction** — Check whether the dependency graph has cycles. Flag any circular dependencies, as they make the module impossible to test in isolation and difficult to reason about.

6. **Assess replaceability** — For each significant dependency, evaluate how hard it would be to swap it out. This reveals hidden architectural constraints.

Constraints:
- Do not suggest changes — this skill produces analysis only.
- Do not conflate "used by" (consumers) with "depends on" (providers) — track the direction correctly.
- Flag transitive dependencies only if they surface as real risks (e.g., a dependency pulls in conflicting versions, or a transitive dep has known vulnerabilities).
- If the codebase is polyglot or uses a microservices architecture, note service-level dependencies separately from code-level ones.

## Inputs

- **Source code**: The module, package, or service under analysis.
- **Dependency manifests**: (Optional) package.json, pom.xml, go.mod, requirements.txt, etc.
- **Architecture context**: (Optional) Known service boundaries, shared databases, or event buses.

## Output

- **Explicit dependency list**: Categorized table of all declared dependencies with version (if known) and purpose.
- **Implicit dependency list**: Bulleted list of hidden dependencies with description and location in code.
- **Side effects inventory**: Per-function table of side effects (reads/writes/emits/calls).
- **Coupling assessment**: Heatmap or table rating each dependency's coupling level (Tight / Moderate / Loose).
- **Circular dependency report**: List of any cycles found, with the full cycle path.
- **Change impact estimate**: If a specific change was provided as context, a ranked list of what would break or need updating.

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

> **Usage note**: Run before `extract-function-module` or `introduce-design-patterns` to understand what you're working with before you restructure it.
