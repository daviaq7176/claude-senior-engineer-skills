# Skill: Memory & CPU Efficiency

## Purpose

Identify and eliminate excessive memory usage, unnecessary CPU work, and resource inefficiencies that degrade throughput and increase infrastructure cost. Use this skill when a service consumes more memory or CPU than expected, when garbage collection pauses are noticeable, or when scaling horizontally doesn't proportionally reduce load.

## Instructions

You are acting as a senior software engineer specializing in resource efficiency. Follow these steps precisely:

1. **Profile before optimizing** — If profiler data is available (heap dumps, CPU flame graphs, allocation traces), read it first and focus on the top consumers. If not, reason from code structure: identify hot loops, high-frequency function calls, and objects with long or unclear lifetimes.

2. **Find memory leaks and retention** — Look for: objects added to collections that are never removed, event listeners or callbacks registered but never unregistered, closures that capture large objects, caches that grow without bounds, and connections or file handles that aren't closed. In long-running services, leaks are cumulative and eventually fatal.

3. **Identify allocation-heavy patterns** — Find code that allocates many small objects in hot paths: string concatenation in loops (use StringBuilder or template literals), creating new arrays/objects in tight loops instead of reusing, excessive wrapping/boxing of primitives, and repeated deep copies of large structures when a reference or shallow copy would work.

4. **Evaluate CPU-intensive patterns** — Find: operations done synchronously that block I/O threads, computation repeated on every request that could be precomputed, string parsing in hot paths that could use binary formats, and regular expression compilation happening per-request instead of once.

5. **Assess data structure efficiency** — Verify data structures match usage patterns: a LinkedList when indexed access is needed, a HashMap with a poor hash function causing collisions, sorted structures when sorting is never needed, or in-memory storage of data that should be streamed.

6. **Review concurrency efficiency** — Find thread or goroutine starvation, excessive context switching, locks held too long (blocking other threads from productive work), and thread pools sized incorrectly for the workload type (CPU-bound vs. I/O-bound work needs different pool configurations).

Constraints:
- Do not optimize based on intuition alone — verify with profiler data or measurable benchmarks.
- Do not trade correctness for performance — a faster implementation that produces wrong results under load is worse than a slow correct one.
- Do not pre-optimize code that runs rarely — focus on hot paths confirmed by measurement.
- Document any non-obvious optimization with a comment explaining why it exists and what it avoids.

## Inputs

- **Source code**: The service or module to analyze.
- **Resource metrics**: (Optional) Memory usage graphs, CPU utilization, GC pause logs, heap dumps.
- **Workload description**: (Optional) Request volume, data size, concurrency level — to contextualize the analysis.

## Output

- **Resource usage assessment**: Current memory and CPU consumption patterns, hot paths identified.
- **Memory findings**: Leaks, unbounded retention, and excessive allocation — each with location and severity.
- **CPU findings**: Unnecessary computation, blocking operations, and inefficient data structures — each with location and estimated impact.
- **Optimization recommendations**: Specific changes ordered by impact, with code examples.
- **Benchmark plan**: How to measure improvement — what to instrument, what metric to track.
- **Expected outcomes**: Estimated reduction in memory or CPU for each recommendation.

---

> **Usage note**: Pair with `analyze-performance-bottlenecks` for full-stack analysis. If the optimization requires algorithm changes, ensure correctness is preserved with the test suite from `automated-test-generation`.
