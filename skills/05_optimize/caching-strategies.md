# Skill: Caching Strategies

## Purpose

Identify opportunities to cache expensive, repeated, or slow operations and implement appropriate caching with correct invalidation, TTLs, and cache key design. Use this skill when repeated computation, redundant database queries, or slow external API calls are degrading performance and the data involved is suitable for caching.

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

---

> **Usage note**: Caching is often the fastest way to improve perceived performance, but it is also a common source of hard-to-debug consistency bugs. Start with the smallest, safest cache (in-process, short TTL) and expand from there.
