# Skill: Fail-Safe Retry Logic

## Purpose

Add robust retry mechanisms, exponential backoff, circuit breakers, and fallback behaviors to operations that can fail transiently — making the system resilient to temporary unavailability of external dependencies. Use this skill when services make calls to external APIs, databases, or queues that occasionally fail and where retrying is safe.

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

You are acting as a senior software engineer implementing production-grade resilience patterns. Follow these steps precisely:

1. **Identify retry candidates** — Find every call to an external system: HTTP API calls, database operations, message queue interactions, file I/O, and cache operations. For each, determine: (a) Is this operation idempotent? (Safe to retry without side effects.) (b) What failure modes are transient? (Network timeout, 503, connection refused.) (c) What failure modes are permanent? (404, 401, 400 — retrying these wastes time and causes confusion.)

2. **Implement exponential backoff with jitter** — For each retry candidate, implement: an initial delay (start small: 100–500ms), exponential growth (double each attempt), a maximum delay cap (avoid waits longer than 30–60s), and jitter (add randomness to avoid thundering herd — all clients retrying simultaneously can overwhelm a recovering service). Never retry with zero delay (tight loop) and never retry with fixed delay (no jitter).

3. **Set retry limits** — Define: maximum number of attempts (typically 3–5 for real-time paths, more for background jobs), total timeout (wall clock limit regardless of attempt count), and per-attempt timeout (don't wait indefinitely for any single attempt).

4. **Implement circuit breakers** — For dependencies that are called frequently, add a circuit breaker: after N consecutive failures, the circuit opens and calls fail immediately without attempting the operation. After a cooldown period, the circuit enters half-open state and allows a test request. This prevents cascading failures when a dependency is down for an extended period.

5. **Design fallbacks** — For each failure scenario, define what the system does when retries are exhausted: return a cached result, return a degraded response (partial data), queue the operation for later processing, surface a user-visible error, or alert an operator. The fallback must be explicitly chosen, not just an unhandled exception.

6. **Ensure idempotency** — Retrying a non-idempotent operation (one that has side effects like creating a record or charging a card) requires idempotency keys. Add idempotency key generation and server-side deduplication before adding retries to any non-idempotent operation.

Constraints:
- Do not add retries to operations that are not idempotent without first making them idempotent.
- Do not retry on client errors (4xx) — these indicate a problem with the request, not the server.
- Do not retry indefinitely in synchronous paths — always bound total retry time.
- Do not swallow the final error after retries are exhausted — surface it clearly.
- Do not implement retry logic inline in business code — use a retry utility or library to keep business logic clean.

## Inputs

- **Source code**: The code making calls to external systems.
- **Dependency characteristics**: (Optional) Known failure modes, SLAs, and rate limits of the dependencies.
- **Availability requirements**: (Optional) What level of availability is required for this operation.

## Output

- **Retry candidate list**: Each external call with: idempotency assessment, transient vs. permanent failures, and retry recommendation.
- **Retry configuration**: For each candidate — initial delay, backoff multiplier, max delay, jitter strategy, max attempts, and total timeout.
- **Circuit breaker configuration**: Threshold, cooldown period, and half-open test strategy for high-frequency dependencies.
- **Fallback behavior**: What happens after retries are exhausted, for each operation.
- **Idempotency additions**: Key generation and deduplication logic for non-idempotent operations that need retries.
- **Implementation code**: Retry wrapper or configuration using the appropriate library (Resilience4j, tenacity, retry-go, axios-retry, etc.).

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

> **Usage note**: Retries without circuit breakers can cause cascading failures. Always implement both. Add `add-logging-monitoring` instrumentation to track retry rates — a high retry rate is a warning signal before the circuit opens.
