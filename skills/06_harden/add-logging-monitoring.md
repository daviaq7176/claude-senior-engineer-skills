# Skill: Add Logging & Monitoring

## Purpose

Add meaningful, structured logs and monitoring hooks to a service so that its behavior in production is observable, debuggable, and alertable. Use this skill when a service is a black box in production, when incidents take too long to diagnose, or when you cannot answer "what is this service doing right now?" from available data.

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

---

> **Usage note**: Observability is foundational. Add this before `fail-safe-retry-logic` — you need to be able to observe retries and failures to tune them correctly. Structured logs enable `reproduce-bug-from-logs` to work effectively.
