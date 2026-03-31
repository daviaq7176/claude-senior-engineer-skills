# Skill: Architecture Review

## Purpose

Evaluate the system design for correctness, scalability, maintainability, and fitness for its stated purpose. Use this skill when designing a new system, reviewing a proposed architecture, or assessing whether an existing architecture can support projected growth.

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

---

> **Usage note**: Architecture reviews are most valuable when done before significant investment is made. Pair with `tradeoff-analysis` to work through contested decisions, and `future-proofing-analysis` to evaluate long-term risk.
