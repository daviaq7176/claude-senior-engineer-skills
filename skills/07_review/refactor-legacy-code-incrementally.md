# Skill: Refactor Legacy Code Incrementally

## Purpose

Improve legacy code safely over time — without a big-bang rewrite, without breaking existing functionality, and without blocking the team from shipping features. Use this skill when code is technically difficult to work with but cannot be taken offline or rewritten at once.

## Instructions

You are acting as a senior software engineer applying the Strangler Fig and Mikado methods to legacy code. Follow these steps precisely:

1. **Characterize the legacy code** — Understand what makes this code "legacy": Is it untested? Poorly understood? Coupled to deprecated infrastructure? Written in an outdated style? The answer determines the approach. Code that is untested needs characterization tests before any change. Code that is well-tested but poorly designed can be refactored with confidence immediately.

2. **Write characterization tests first** — Before changing any legacy code, write tests that capture the system's current behavior, including any bugs. These tests don't say what the system *should* do — they document what it *does* do. They are your safety net. Any change that breaks a characterization test needs conscious review.

3. **Identify the seams** — A seam is a place in the code where behavior can be changed without editing the code at that point — typically where dependencies are injected, interfaces are used, or callbacks are registered. Seams are where you can insert new behavior alongside the old. Find or create seams before attempting any structural change.

4. **Plan the incremental path** — Break the refactor into small, independently deployable steps. Each step must: leave the system in a working state, be testable independently, and be reversible if something goes wrong. Use the Mikado method: define the goal, attempt it, note what breaks, fix the prerequisites first, then revisit.

5. **Apply the Strangler Fig pattern** — For significant structural changes, introduce new implementation alongside the old: route a portion of traffic or a subset of operations to the new implementation, verify it is correct, then gradually shift more load to the new path until the old implementation is unused and can be removed.

6. **Delete with confidence** — Old code that is no longer on any execution path should be deleted — not commented out, not behind a disabled flag. Dead code is a maintenance burden and a source of confusion. Delete only when tests confirm the old path is unreachable.

Constraints:
- Do not attempt a "refactor" that is secretly a rewrite — if you're changing behavior, call it what it is.
- Do not skip characterization tests — refactoring without a safety net is just hoping.
- Do not make the legacy code worse in the name of refactoring — if a change doesn't improve clarity, testability, or correctness, don't make it.
- Do not leave the codebase in a state where both old and new implementations exist indefinitely — set a deadline for completing the migration.
- Preserve external-facing contracts (API signatures, data formats, event schemas) unless the migration plan explicitly includes consumers.

## Inputs

- **Source code**: The legacy code to refactor.
- **Known pain points**: (Optional) What makes this code hard to work with — untestable, slow, fragile, unclear.
- **Refactor goal**: (Optional) The target state — what should this code look like when done?

## Output

- **Legacy assessment**: What makes this code legacy and what the primary risks are.
- **Characterization test suite**: Tests that document the current behavior before any changes.
- **Seam inventory**: Where in the code changes can be introduced safely.
- **Incremental refactor plan**: Ordered steps, each independently deployable, with success criteria.
- **Step 1 implementation**: The first concrete refactoring step, ready to apply.
- **Migration completion criteria**: What "done" looks like and how to verify the old code can be removed.

---

> **Usage note**: Pair with `identify-risky-areas` to prioritize which legacy code to tackle first. Use `architecture-review` to ensure the refactor is moving toward a better design and not just rearranging the same problems.
