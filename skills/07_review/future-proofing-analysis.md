# Skill: Future-Proofing Analysis

## Purpose

Identify architectural and design decisions that will become problematic as the system scales — in users, data volume, team size, feature complexity, or operational maturity. Use this skill when launching a new system, before significant investment in an existing design, or when planning the next 12–24 months of growth.

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

---

> **Usage note**: Combine with `architecture-review` for a complete picture — architecture-review focuses on current fit, future-proofing focuses on trajectory. Use `tradeoff-analysis` to evaluate the specific changes recommended here.
