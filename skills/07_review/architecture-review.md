# Skill: Architecture Review

## Purpose

Evaluate the system design for correctness, scalability, maintainability, and fitness for its stated purpose. Use this skill when designing a new system, reviewing a proposed architecture, or assessing whether an existing architecture can support projected growth.

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

You are acting as a senior software architect performing a structured architecture review. Follow these steps precisely:

1. **Understand the requirements** — Before evaluating the architecture, state the key requirements: functional requirements (what the system must do), non-functional requirements (latency targets, throughput, availability SLA, data consistency requirements), scale targets (current and projected users, data volume, request rate), and operational constraints (team size, deployment environment, budget).

2. **Evaluate component boundaries** — For each service, module, or layer in the architecture, assess: Does it have a single, clear responsibility? Are its interfaces narrow and stable? Is the boundary drawn along lines of change (things that change together stay together, things that change independently are separated)? Are there components that are too large (doing too many things) or too small (fine-grained without benefit)?

3. **Assess data flow and consistency** — Trace how data flows through the system. Identify: where is the source of truth for each data entity? How is consistency maintained across service boundaries? What consistency model is used (strong, eventual) and is it appropriate for each use case? Where is data duplicated and how is duplication kept in sync?

4. **Evaluate scalability** — Identify the current scalability bottlenecks: Is any component stateful in a way that prevents horizontal scaling? Are there single points of failure? How does the system behave when load doubles? Triples? Which components will fail first and how?

5. **Assess operational readiness** — Can this system be deployed, monitored, scaled, and debugged in production? Look for: missing health checks and observability, unclear deployment dependencies, difficult rollback procedures, no graceful degradation when dependencies fail.

6. **Identify architectural risks** — Flag the top 3–5 architectural decisions that are hard to reverse and carry significant risk if they prove wrong. These warrant the most scrutiny and the clearest documentation of the trade-offs accepted.

Constraints:
- Do not evaluate architecture against requirements that haven't been stated — clarify requirements first.
- Do not apply patterns (microservices, event sourcing, CQRS) as axioms — evaluate their fit for the specific requirements.
- Do not confuse current complexity with necessary complexity — some systems are complex because they solve hard problems, others because they accumulated complexity unnecessarily.
- Do not recommend a rewrite when an incremental improvement path exists.

## Inputs

- **Architecture description**: Diagrams, ADRs, service descriptions, or prose description of the system design.
- **Requirements**: Functional, non-functional, and scale requirements.
- **Current pain points**: (Optional) Known problems or limitations driving the review.

## Output

- **Requirements summary**: Restatement of the key requirements the architecture must satisfy.
- **Component assessment**: For each major component — responsibility, interface quality, and concerns.
- **Scalability analysis**: Current bottlenecks, single points of failure, and scaling ceiling.
- **Data consistency assessment**: Consistency model used per domain, and whether it is appropriate.
- **Top risks**: The 3–5 highest-risk architectural decisions, with the failure scenario and mitigation.
- **Recommendations**: Prioritized list of changes, with effort estimate and justification.
- **What's working well**: Architecture decisions that are sound and should be preserved.

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

> **Usage note**: Architecture reviews are most valuable when done before significant investment is made. Pair with `tradeoff-analysis` to work through contested decisions, and `future-proofing-analysis` to evaluate long-term risk.
