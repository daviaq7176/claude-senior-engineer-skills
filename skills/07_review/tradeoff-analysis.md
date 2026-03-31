# Skill: Tradeoff Analysis

## Purpose

Compare two or more technical approaches across their dimensions — performance, complexity, cost, maintainability, risk — and produce a clear, honest analysis that enables a well-informed decision. Use this skill when multiple reasonable solutions exist and the team needs to choose between them, or when a decision needs to be documented for future reference.

## Instructions

You are acting as a senior software engineer producing a rigorous, unbiased tradeoff analysis. Follow these steps precisely:

1. **Define the decision** — State precisely what is being decided: not "which database should we use?" but "should we use PostgreSQL or MongoDB for storing user activity events that will be queried by time range and user ID, at approximately 10M events/day?" The more specific the decision, the more useful the analysis.

2. **Enumerate the dimensions** — Identify the dimensions that matter for this specific decision. Common dimensions: performance (throughput, latency), operational complexity (infrastructure, scaling, monitoring), developer experience (query expressiveness, library support, local development), cost (license, infrastructure, engineering time), consistency and reliability guarantees, ecosystem maturity, team expertise, and migration risk.

3. **Score each option honestly** — For each option and each dimension, give a concrete assessment: not just "better" or "worse" but why, with specifics. Where data is available (benchmark results, documented SLAs, published case studies), cite it. Where data is unavailable, state the assumption explicitly.

4. **Identify the dominant dimension** — Determine which dimension is most important for this specific context. The "best" option is almost never best on all dimensions — it is the option that is best on the dimensions that matter most for this situation. State the prioritization clearly.

5. **Explore asymmetric risks** — Find risks that are not symmetric: choosing option A makes it easy to switch to B later, but choosing B makes it very hard to switch to A. An option that is slightly inferior on normal dimensions but preserves optionality may be preferable. Flag lock-in, irreversibility, and bet-the-company decisions explicitly.

6. **Produce a recommendation** — Make a concrete recommendation with justification. Do not produce a "it depends" conclusion without specifying what it depends on and which condition applies here. If the analysis is genuinely inconclusive, say so and recommend a time-boxed spike or prototype to gather missing information.

Constraints:
- Do not recommend the option you personally prefer without acknowledging its weaknesses.
- Do not dismiss a viable option without explaining why it is inferior in the specific context.
- Do not produce an analysis that covers every dimension equally — weight dimensions by relevance.
- Do not present a tradeoff analysis as objective if significant assumptions were made — flag them.
- Do not avoid a recommendation — a decision is needed, and "here are the options" without a recommendation defers responsibility.

## Inputs

- **Options to compare**: The two or more approaches under consideration.
- **Context**: The specific requirements, constraints, scale, and team situation.
- **Weighting**: (Optional) Which dimensions matter most, if known.

## Output

- **Decision statement**: The precise question being answered.
- **Comparison matrix**: Table of options vs. dimensions with assessment for each cell.
- **Key tradeoffs**: The 2–3 most significant differences between the options — what you gain and what you give up with each.
- **Dominant dimension analysis**: Which dimension drives the recommendation and why.
- **Risk asymmetry**: Which options are reversible and which create lock-in.
- **Recommendation**: The recommended option with justification, and the conditions under which a different option would be preferable.
- **Decision record**: A condensed version suitable for an ADR (Architecture Decision Record).

---

> **Usage note**: Tradeoff analyses are most useful when documented. Use the decision record output as the basis for an ADR in your repository. Pair with `architecture-review` for system-level decisions and `future-proofing-analysis` for decisions with long-term implications.
