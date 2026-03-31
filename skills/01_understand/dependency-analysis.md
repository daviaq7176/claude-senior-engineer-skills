# Skill: Dependency Analysis

## Purpose

Identify all dependencies a module has — internal and external — including hidden side effects, shared state, and coupling that doesn't appear in import statements. Use this skill before refactoring, extracting a module, changing an interface, or assessing the blast radius of a change.

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

---

> **Usage note**: Run before `extract-function-module` or `introduce-design-patterns` to understand what you're working with before you restructure it.
