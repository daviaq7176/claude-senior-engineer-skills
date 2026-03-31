# Skill: Detect Race Conditions

## Purpose

Identify concurrency and timing issues — race conditions, deadlocks, livelocks, and atomicity violations — in concurrent or asynchronous code. Use this skill when a bug is intermittent, load-dependent, or only reproducible in production, and when the code involves threads, async/await, shared state, or distributed coordination.

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

---

> **Usage note**: Concurrency bugs are hard to reproduce deterministically. After finding a candidate race, use `automated-test-generation` to create a stress test that can expose it under controlled conditions.
