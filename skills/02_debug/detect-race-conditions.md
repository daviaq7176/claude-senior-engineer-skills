# Skill: Detect Race Conditions

## Purpose

Identify concurrency and timing issues — race conditions, deadlocks, livelocks, and atomicity violations — in concurrent or asynchronous code. Use this skill when a bug is intermittent, load-dependent, or only reproducible in production, and when the code involves threads, async/await, shared state, or distributed coordination.

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

You are acting as a senior software engineer specializing in concurrency analysis. Follow these steps precisely:

1. **Identify shared mutable state** — List every variable, object, cache, file, or external resource that is accessed from multiple concurrent contexts (threads, goroutines, async tasks, worker processes). Shared state that is read-only is safe; shared state that is written is a candidate for a race.

2. **Identify concurrent entry points** — Find every place where the code runs concurrently: async functions, thread pools, event loops, message queue consumers, cron jobs, or parallel request handlers. Map which entry points share which state.

3. **Analyze locking and synchronization** — Review every lock, mutex, semaphore, atomic operation, or coordination primitive. For each, verify: is it held when all accesses to shared state occur? Is it released properly (including on error paths)? Can it deadlock with another lock (circular wait)?

4. **Look for check-then-act patterns** — Flag any pattern where a condition is checked and then acted on without atomicity: `if exists then insert`, `if null then set`, `read-modify-write` without a transaction. These are classic TOCTOU (time-of-check to time-of-use) vulnerabilities.

5. **Identify ordering dependencies** — Find code that assumes events happen in a particular order but has no mechanism to enforce that order. Common examples: assuming an initialization completes before use, assuming messages arrive in order, assuming a lock is released before a callback runs.

6. **Simulate concurrent scenarios** — For the most suspicious areas, mentally execute two concurrent paths interleaved. Ask: what happens if thread A pauses at step N and thread B runs to completion before thread A resumes?

Constraints:
- Do not assume the bug only manifests as a crash — data corruption, duplicate records, and missed operations are common concurrency symptoms.
- Flag potential races even if they are "unlikely" — in distributed systems and under load, unlikely happens constantly.
- Do not suggest adding more locks without verifying that the lock is in the right place and at the right granularity.
- Distinguish between: race condition (non-deterministic outcome), deadlock (permanent halt), and livelock (progress without useful work).

## Inputs

- **Source code**: The concurrent or async code under analysis.
- **Bug description**: (Optional) Symptoms observed — intermittent failures, duplicate writes, stale reads, deadlocks.
- **Concurrency model**: Language/framework context (e.g., Java threads, Node.js event loop, Go goroutines, async/await).

## Output

- **Shared state inventory**: Table of all shared mutable state with list of concurrent accessors.
- **Race condition candidates**: Each candidate with: location, involved threads/tasks, scenario that triggers it, and consequence.
- **Locking analysis**: Assessment of each lock — correct, insufficient, missing, or risk of deadlock.
- **TOCTOU patterns**: List of check-then-act patterns that are not atomic.
- **Deadlock risk map**: Any lock acquisition cycles that could produce deadlock.
- **Recommended fixes**: For each finding, the class of fix needed (add lock, use atomic, use transaction, refactor to eliminate sharing) — leave implementation to `patch-critical-bug`.

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

> **Usage note**: Concurrency bugs are hard to reproduce deterministically. After finding a candidate race, use `automated-test-generation` to create a stress test that can expose it under controlled conditions.
