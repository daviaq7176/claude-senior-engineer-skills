# Skill: Future-Proofing Analysis

## Purpose

Identify architectural and design decisions that will become problematic as the system scales — in users, data volume, team size, feature complexity, or operational maturity. Use this skill when launching a new system, before significant investment in an existing design, or when planning the next 12–24 months of growth.

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

You are acting as a senior software engineer and technical strategist performing a forward-looking risk assessment. Follow these steps precisely:

1. **Define the scaling vectors** — Identify the dimensions along which this system is expected to grow: user count, request volume, data volume, number of engineers on the team, number of features, geographic distribution, regulatory requirements, third-party integrations. Not all systems scale on all dimensions — focus on the ones most relevant to this context.

2. **Stress-test the architecture** — For each scaling vector, project the system's behavior at 10x and 100x current scale. Ask: which components will fail first? What will the bottleneck be? What design decision will be the most painful to undo? Look specifically at: stateful components that can't be horizontally scaled, monolithic data models that will require complex migrations, tight coupling that prevents independent scaling, and operational processes that require human intervention at scale.

3. **Identify irreversible decisions** — Surface the decisions that are easy to make today but expensive to change later: data model choices (adding columns is easy, splitting tables is hard), API contracts with external consumers (versioning after the fact is painful), consistency models (moving from eventual to strong consistency is very hard), and technology choices with significant migration costs (changing your message queue, your ORM, your auth provider).

4. **Evaluate team scalability** — As teams grow, architecture that is clear for one team becomes chaotic for five. Assess: Can the codebase support multiple teams working without constant coordination? Are service boundaries drawn in ways that allow parallel development? Is the system's behavior documented well enough that new engineers can understand it without institutional knowledge?

5. **Identify accumulating technical debt** — Find decisions that create incremental debt with each use: copy-pasted patterns instead of abstractions (every new feature copies the pattern and adds drift), inconsistent data access patterns (some code uses the ORM, some uses raw queries), and manual processes that will need automation (deployment steps, data migrations, configuration changes).

6. **Produce a risk-prioritized roadmap** — Rank risks by: likelihood of becoming a real problem (given realistic growth projections), cost to fix later vs. now, and effort required. Some risks are worth accepting; others should be addressed before they compound.

Constraints:
- Do not over-engineer for scale that is not realistic — scaling a system for 10B users when 100K is ambitious is waste, not prudence.
- Do not conflate current complexity with future risk — some complexity today prevents complexity tomorrow.
- Do not recommend rewrites — identify incremental improvements that can be made alongside ongoing development.
- Ground all projections in realistic growth assumptions — state the assumptions explicitly.

## Inputs

- **Architecture or codebase**: The system to analyze.
- **Growth projections**: Expected scale changes over the next 12–24 months.
- **Current pain points**: (Optional) Existing friction that suggests where stress will accumulate.

## Output

- **Scaling vector assessment**: For each growth dimension, projected impact on the system.
- **Top 5 future risks**: The most likely and most painful problems at scale, ranked by combined likelihood and impact.
- **Irreversible decision inventory**: Decisions made today that will be expensive to change later, with current change-cost vs. future change-cost.
- **Team scaling assessment**: Architecture readiness for team growth.
- **Technical debt trajectory**: Areas where debt is accumulating per-feature and will compound.
- **Prioritized roadmap**: Recommended improvements, ordered by risk reduction per unit of effort.

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

> **Usage note**: Combine with `architecture-review` for a complete picture — architecture-review focuses on current fit, future-proofing focuses on trajectory. Use `tradeoff-analysis` to evaluate the specific changes recommended here.
