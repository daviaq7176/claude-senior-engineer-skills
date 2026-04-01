# Skill: Add Logging & Monitoring

## Purpose

Add meaningful, structured logs and monitoring hooks to a service so that its behavior in production is observable, debuggable, and alertable. Use this skill when a service is a black box in production, when incidents take too long to diagnose, or when you cannot answer "what is this service doing right now?" from available data.

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

You are acting as a senior software engineer adding observability to a production service. Follow these steps precisely:

1. **Identify observability gaps** — Determine what questions you cannot currently answer about this service in production: Why did this request fail? How long did this operation take? Which code path ran? How many times did this error occur in the last hour? Each unanswered question is a logging or monitoring gap.

2. **Add structured logging** — Replace unstructured string logs with structured key-value logs (JSON). Every log entry should include: timestamp, log level, correlation/trace ID (so logs from the same request can be grouped), service/module name, and event-specific fields. Do not log by concatenating strings — use a structured logger.

3. **Log at the right level** — Apply log levels consistently: ERROR for failures that require human attention, WARN for recoverable problems or unexpected-but-handled conditions, INFO for significant business events (user logged in, payment processed, job completed), DEBUG for detailed diagnostic data (not enabled in production by default).

4. **Instrument key operations** — Add timing and outcome logging around: external service calls (database, HTTP, queue), authentication and authorization decisions, job/task start and completion, and business-critical operations (order placed, payment charged, email sent). Include: operation name, duration, outcome (success/failure), and relevant IDs.

5. **Add health and metrics endpoints** — If not present, add: a health check endpoint that tests real dependencies (DB connection, queue connectivity), metrics endpoints or instrumentation for a metrics collector (Prometheus, StatsD, CloudWatch), and counters/histograms for: request count by status code, request duration, queue depth, error rate.

6. **Define alerting thresholds** — For each metric added, specify: what value should trigger an alert, at what severity (page vs. notify), and for what duration (sustained for 5 minutes vs. single occurrence).

Constraints:
- Do not log sensitive data — no passwords, tokens, full credit card numbers, or PII without masking.
- Do not log at ERROR level for expected failures (user not found, validation error) — these should be INFO or WARN.
- Do not add so much logging that the log volume makes logs useless — every log line should answer a question.
- Do not log inside tight loops without rate limiting — excessive logging degrades performance.
- Correlation IDs must be propagated through all downstream calls — a trace that stops at the service boundary is useless.

## Inputs

- **Source code**: The service or module to instrument.
- **Incident history**: (Optional) Past incidents and the questions that were hard to answer during them.
- **Existing observability stack**: (Optional) Log aggregator, metrics system, or tracing infrastructure already in use.

## Output

- **Observability gap analysis**: Questions that cannot currently be answered from logs/metrics, and what to add to answer them.
- **Logging additions**: Specific log statements to add, with level, fields, and location.
- **Metrics instrumentation**: Counters, histograms, and gauges to add, with names following existing conventions.
- **Health check implementation**: Health endpoint code if not present.
- **Alerting definitions**: Metric thresholds and conditions for alerts, in prose or as alert rule format.
- **Sensitive data audit**: Any existing logs that contain sensitive data and must be redacted.

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

> **Usage note**: Observability is foundational. Add this before `fail-safe-retry-logic` — you need to be able to observe retries and failures to tune them correctly. Structured logs enable `reproduce-bug-from-logs` to work effectively.
