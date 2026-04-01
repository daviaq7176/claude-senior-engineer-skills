# Skill: Refactor Legacy Code Incrementally

## Purpose

Improve legacy code safely over time — without a big-bang rewrite, without breaking existing functionality, and without blocking the team from shipping features. Use this skill when code is technically difficult to work with but cannot be taken offline or rewritten at once.

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

> **Usage note**: Pair with `identify-risky-areas` to prioritize which legacy code to tackle first. Use `architecture-review` to ensure the refactor is moving toward a better design and not just rearranging the same problems.
