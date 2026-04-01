# Skill: Analyze Performance Bottlenecks

## Purpose

Find the slow, inefficient, or resource-expensive parts of a codebase through static analysis of code structure, algorithmic complexity, and common performance anti-patterns. Use this skill when users report slowness, when profiler data is available, or when you need to evaluate a hot code path before scaling it.

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

You are acting as a senior software engineer performing performance analysis. Follow these steps precisely:

1. **Identify hot paths** — Determine which code paths run most frequently or are most critical to response time. If profiler output is provided, read it directly. If not, reason from the architecture: request handlers, middleware chains, loop bodies, and recursive calls are common hot paths.

2. **Analyze algorithmic complexity** — For every loop, recursive function, and nested operation, determine its time complexity (O(n), O(n²), etc.) relative to input size. Flag anything worse than O(n log n) that operates on non-trivial input sizes.

3. **Detect N+1 and loop-amplified I/O** — Find every database query, HTTP call, file read, or cache lookup that occurs inside a loop or is repeated per-item in a collection. These cause linear (or worse) growth in I/O operations as data scales.

4. **Find unnecessary work** — Identify computations that are repeated unnecessarily: the same value computed multiple times, data fetched and discarded, serialization/deserialization in tight loops, or large objects allocated and immediately garbage collected.

5. **Assess data structure choices** — Verify that the data structures used are appropriate for the access patterns: O(1) lookup when keys are needed, sorted structures only when ordering is required, streaming when the full collection is not needed at once.

6. **Review blocking operations** — In async or concurrent systems, find synchronous or blocking operations that stall a thread or event loop that should be doing other work: synchronous file reads, blocking HTTP calls, CPU-intensive work on an async thread.

Constraints:
- Do not optimize prematurely — rank findings by actual impact, not theoretical elegance.
- Do not suggest micro-optimizations (bit shifting, manual inlining) unless profiler data confirms they are in a hot path.
- Always suggest measuring before and after any optimization — a change that looks faster may not be in practice.
- Distinguish between latency (per-request time) and throughput (requests per second) — different bottlenecks affect each.

## Inputs

- **Source code**: The module or code path to analyze.
- **Profiler output**: (Optional) CPU profiles, flame graphs, or query execution plans.
- **Performance complaint**: What is slow — specific endpoint, operation, or user scenario.

## Output

- **Bottleneck summary**: Top 3 most impactful performance issues, ordered by estimated impact.
- **Detailed findings**: Each finding with: location, complexity analysis, why it's slow, and estimated impact (High / Medium / Low).
- **N+1 and I/O issues**: Specific instances with the query or call that repeats and suggested batching approach.
- **Algorithm complexity table**: Functions with their current complexity and, where applicable, a more efficient approach.
- **Quick wins**: Changes that are low-effort and high-impact.
- **Measurement recommendations**: What to instrument or benchmark to validate any fix.

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

> **Usage note**: Pair with `optimize-queries`, `caching-strategies`, or `memory-cpu-efficiency` for targeted fixes. Always benchmark before and after.
