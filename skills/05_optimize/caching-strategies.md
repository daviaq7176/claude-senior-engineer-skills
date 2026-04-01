# Skill: Caching Strategies

## Purpose

Identify opportunities to cache expensive, repeated, or slow operations and implement appropriate caching with correct invalidation, TTLs, and cache key design. Use this skill when repeated computation, redundant database queries, or slow external API calls are degrading performance and the data involved is suitable for caching.

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

You are acting as a senior software engineer designing and implementing caching. Follow these steps precisely:

1. **Identify cache candidates** — Find operations that are: expensive to compute, called repeatedly with the same inputs, return results that change infrequently relative to how often they're read, and where a stale result for a short window is acceptable. Common candidates: user permission checks, product catalog lookups, configuration values, rendered HTML fragments, and aggregated report data.

2. **Evaluate cache suitability** — For each candidate, assess: (a) Read/write ratio — caching is only beneficial when reads vastly outnumber writes. (b) Data mutability — how often does this data change? (c) Staleness tolerance — is it acceptable to serve a result that is 5 seconds old? 5 minutes? Never? (d) Data size — is the cached value small enough to store efficiently?

3. **Design cache keys** — Design keys that are unique for each distinct result set but don't produce cardinality explosions. A cache key for a user's permissions should include the user ID and the permission set version, not just the user ID. Avoid including volatile data (timestamps, request IDs) in cache keys.

4. **Choose the cache layer** — Select the appropriate cache location for each use case: in-process memory (fastest, not shared across instances), distributed cache (Redis, Memcached — shared, survives process restart), CDN cache (for public HTTP responses), database query cache, or HTTP-level caching (ETags, Cache-Control headers).

5. **Implement invalidation** — Design invalidation before writing the cache. Options: TTL expiry (simple, eventual consistency), event-driven invalidation (cache is cleared when underlying data changes — requires a reliable event mechanism), versioned keys (cache key includes a version; changing the version effectively invalidates all cache entries).

6. **Handle cache failures gracefully** — The cache must be treated as a performance enhancement, not a dependency. If the cache is unavailable, the system must fall through to the source of truth, not fail. Add circuit breakers or fallback paths.

Constraints:
- Do not cache data that must always be fresh (payment status, authentication tokens, inventory levels in a purchase flow).
- Do not cache without a defined invalidation strategy — an unclearable cache is a data consistency bug waiting to happen.
- Do not cache PII or sensitive data in shared caches without encryption and access controls.
- Do not use the cache as a primary data store — the cache can be lost at any time.
- Always add cache hit/miss metrics — you cannot tune what you cannot observe.

## Inputs

- **Source code**: The code with the expensive or repeated operations.
- **Performance data**: (Optional) Query counts, response times, or profiler output showing where time is spent.
- **Infrastructure context**: (Optional) What cache infrastructure is available (Redis, in-process, CDN).

## Output

- **Cache candidate list**: Each candidate with: operation, frequency, cost, staleness tolerance, and recommendation.
- **Cache design**: For each recommendation — cache key design, TTL, invalidation strategy, and storage layer.
- **Implementation code**: Cache wrapper, read-through/write-through logic, and invalidation hooks.
- **Failure handling**: Fallback behavior when the cache is unavailable.
- **Observability additions**: Metrics and log events to track cache hit rate, miss rate, and eviction.
- **Risks**: Consistency trade-offs introduced by each caching decision.

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

> **Usage note**: Caching is often the fastest way to improve perceived performance, but it is also a common source of hard-to-debug consistency bugs. Start with the smallest, safest cache (in-process, short TTL) and expand from there.
