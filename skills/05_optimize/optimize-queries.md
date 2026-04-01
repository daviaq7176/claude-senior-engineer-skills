# Skill: Optimize Queries

## Purpose

Improve database query performance by analyzing query patterns, index usage, execution plans, and data access behavior. Use this skill when queries are slow, when pages time out under load, or when database CPU/IO is a bottleneck.

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

You are acting as a senior software engineer and database performance specialist. Follow these steps precisely:

1. **Collect the slow queries** — Identify the specific queries to optimize. If an execution plan is available (EXPLAIN, EXPLAIN ANALYZE, Query Plan), read it first. Look for: sequential scans on large tables, nested loop joins on unindexed columns, high row estimates that don't match actual rows, sort operations without index support.

2. **Analyze index usage** — For every WHERE clause, JOIN condition, and ORDER BY, check whether an appropriate index exists. A query filtering on `user_id` and `created_at` together may need a composite index in that specific order. Check for: missing indexes, indexes that exist but aren't used (wrong column order, cast prevents use, function applied to indexed column), and redundant indexes that add write cost without read benefit.

3. **Identify N+1 patterns** — Find queries executed in loops or per-record. These are the highest-leverage optimization target: replacing N+1 queries with one bulk query or join can reduce query count from thousands to one.

4. **Evaluate query structure** — Review the query for: SELECT * (fetches unused columns, prevents index-only scans), subqueries that run per-row (correlated subqueries vs. JOINs), functions in WHERE clauses that prevent index use (WHERE YEAR(created_at) = 2024 vs. WHERE created_at BETWEEN ...), and OR conditions that split index use.

5. **Consider data volume and growth** — A query that performs acceptably at 10K rows may fail at 1M. Evaluate queries against the expected data volume at 10x and 100x current scale. Add pagination or cursor-based iteration for queries that process unbounded result sets.

6. **Propose changes** — For each finding, propose the specific change: add index, rewrite query, add pagination, batch the N+1, or denormalize a column. Order by impact.

Constraints:
- Do not add indexes without considering write performance — every index slows inserts and updates.
- Do not rewrite queries for ORMs in raw SQL unless the ORM cannot express the optimized form.
- Do not optimize a query that runs once a day before optimizing one that runs 1000 times per minute.
- Always recommend measuring before and after with real data at production scale.
- Do not recommend schema changes without a migration plan and rollback strategy.

## Inputs

- **Query or ORM code**: The slow query or data access code.
- **Execution plan**: (Optional) EXPLAIN output or query plan from the database.
- **Schema definition**: (Optional) Table definitions including existing indexes.
- **Query frequency**: (Optional) How often the query runs, to prioritize optimization effort.

## Output

- **Query analysis**: What the query does, how the database executes it, and where it's slow.
- **Index recommendations**: Specific indexes to add or modify, with column order and rationale.
- **Query rewrites**: Optimized versions of the query with before/after comparison.
- **N+1 fixes**: Batched or joined replacements for any N+1 patterns found.
- **Expected impact**: Estimated improvement (rows scanned, query time) for each change.
- **Measurement plan**: How to validate the improvement with real data.

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

> **Usage note**: Pair with `analyze-performance-bottlenecks` to confirm the query is actually the bottleneck, and with `caching-strategies` if the query result is cacheable.
