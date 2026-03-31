# Skill: Analyze Performance Bottlenecks

## Purpose

Find the slow, inefficient, or resource-expensive parts of a codebase through static analysis of code structure, algorithmic complexity, and common performance anti-patterns. Use this skill when users report slowness, when profiler data is available, or when you need to evaluate a hot code path before scaling it.

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

---

> **Usage note**: Pair with `optimize-queries`, `caching-strategies`, or `memory-cpu-efficiency` for targeted fixes. Always benchmark before and after.
