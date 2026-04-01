# Skill: Trace Execution Path

## Purpose

Follow the execution flow of a program for a specific input or scenario, with special attention to asynchronous paths, callbacks, and non-obvious control flow. Use this skill when you know a bug exists but need to understand exactly which code ran (and in what order) to produce it.

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

You are acting as a senior software engineer performing execution tracing. Follow these steps precisely:

1. **Define the entry point and trigger** — Establish the exact starting point of the execution (e.g., HTTP request, event fired, scheduled job, function call) and the inputs that trigger the path under investigation.

2. **Walk the synchronous path** — Trace through the code step by step in execution order. For each function call, note: what it receives, what it does, and what it returns or mutates. Do not skip "obvious" steps — the bug is often in the obvious ones.

3. **Handle async boundaries explicitly** — At every `await`, `Promise`, callback, event listener, goroutine, or thread boundary, explicitly note: what is scheduled, when it runs relative to the rest, and what state it captures at the time it is created (closures, references, copies).

4. **Track mutable state** — Maintain a running record of all variables that change throughout execution. Note their value at each step. Bugs often appear where a variable has an unexpected value because of an earlier step that wasn't considered.

5. **Identify the divergence point** — Find where the actual behavior deviates from the expected behavior. This is the key output of the trace.

6. **Verify assumptions** — List any assumption the code makes at the divergence point (about types, ordering, previous state, API responses) and determine whether that assumption holds for the input under investigation.

Constraints:
- Trace the actual path for the specific input, not the general case.
- Do not skip async steps — they are the most common source of hard-to-find bugs.
- When tracing through library or framework code, stay at the boundary — describe what the library does at a functional level rather than diving into its implementation.
- If a path depends on runtime state you can't know statically (e.g., DB value, env var), note the assumption and both branches.

## Inputs

- **Source code**: The functions involved in the execution path.
- **Trigger input**: The exact input, request, or event that triggers the path.
- **Expected vs. actual behavior**: What should happen vs. what does happen.

## Output

- **Execution trace**: Numbered step-by-step sequence of function calls with inputs/outputs at each step.
- **Async timeline**: If async is involved, a timeline showing what runs when relative to other operations.
- **State snapshots**: Key variable values at critical points in the trace.
- **Divergence point**: The exact step where behavior deviates, with explanation.
- **Broken assumption**: The specific assumption that doesn't hold for the given input.
- **Fix direction**: A brief pointer toward what needs to change (leave the actual fix to `patch-critical-bug`).

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

> **Usage note**: Use this skill after `reproduce-bug-from-logs` gives you a reproduction scenario. The trace is the bridge between "I can reproduce it" and "I know how to fix it."
