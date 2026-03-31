# Skill: Reproduce Bug from Logs

## Purpose

Reconstruct the exact sequence of events that led to a bug using log output, stack traces, error messages, and any available telemetry. Use this skill when a bug is reported in production and you need to understand what happened before attempting a fix.

## Instructions

You are acting as a senior software engineer performing incident analysis. Follow these steps precisely:

1. **Parse the timeline** — Extract all log entries relevant to the bug. Sort them chronologically. Identify the first anomalous event — the earliest sign that something was wrong, which is often not the same as the first visible error.

2. **Reconstruct the request or event chain** — Using correlation IDs, session IDs, user IDs, or timestamps, trace the sequence of operations that led to the failure. Identify which service, function, or handler was active at each step.

3. **Identify the failure point** — Determine exactly where the system deviated from expected behavior. Distinguish between: (a) the root cause (where things went wrong), (b) the propagation path (how the error spread), and (c) the symptom (what the user or monitor saw).

4. **Extract reproduction conditions** — From the log context, determine what inputs, state, or external conditions were present at the time of failure. Ask: what would need to be true to trigger this again?

5. **Draft a reproduction scenario** — Write a step-by-step reproduction script or description that could be used to trigger the bug in a test environment. Include: preconditions, exact inputs, sequence of actions, and expected vs. actual outcome.

6. **Assess confidence** — Rate how confident you are in the reproduction scenario and what gaps remain (e.g., missing log context, log sampling, async gaps between services).

Constraints:
- Do not assume a cause — derive everything from the logs. State clearly when you are inferring vs. observing.
- Do not omit log lines that seem irrelevant — they may be important context.
- Flag log gaps (time gaps, missing correlation IDs, truncated stack traces) explicitly.
- If multiple plausible causes exist, list each with a probability estimate based on the evidence.

## Inputs

- **Log output**: Raw logs, stack traces, error messages, or structured log JSON.
- **Incident report**: (Optional) What the user or monitor observed, time of occurrence, affected users.
- **Service topology**: (Optional) Architecture context to help correlate logs across services.

## Output

- **Timeline**: Chronological list of events from logs with timestamps, service, and event description.
- **Root cause hypothesis**: The most likely cause of the failure, with evidence from the logs.
- **Propagation path**: How the error spread from root cause to visible symptom.
- **Reproduction scenario**: Step-by-step instructions to reproduce the bug, including preconditions.
- **Confidence assessment**: High / Medium / Low, with a list of gaps or uncertainties.
- **Suggested next steps**: What additional logging, metrics, or tests would confirm or refute the hypothesis.

---

> **Usage note**: Hand off the reproduction scenario to `trace-execution-path` for deeper code-level analysis, and to `patch-critical-bug` for the fix. Document the timeline in your postmortem.
