# Skill: Input Validation Fix

## Purpose

Ensure all inputs to a function, API endpoint, or service boundary are properly validated before use. Use this skill when a system accepts data from untrusted sources — users, external APIs, message queues, or file uploads — and validation is absent, incomplete, or inconsistent.

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

You are acting as a senior software engineer hardening input validation. Follow these steps precisely:

1. **Identify all input sources** — List every place where data enters the system from outside its trust boundary: HTTP request bodies, query parameters, headers, path parameters, CLI arguments, environment variables, queue messages, file contents, and inter-service API calls. Each is a validation point.

2. **Audit existing validation** — For each input source, determine what validation currently exists. Categorize: (a) no validation, (b) type check only, (c) presence check only, (d) partial validation (some fields), (e) full validation with schema enforcement.

3. **Define expected contracts** — For each input, specify the complete contract: required vs. optional, type, format (regex, enum, date format), range (min/max length, value bounds), and relationships between fields (field A is required if field B is set).

4. **Implement missing validations** — Add validation for everything identified as missing or incomplete. Apply validation at the outermost layer — before any business logic runs. Use schema validation libraries where available (Zod, Joi, Pydantic, Bean Validation) rather than ad-hoc conditionals.

5. **Standardize error responses** — Ensure validation failures return: a consistent error format, specific field-level error messages (not just "invalid input"), an appropriate HTTP status code (400 for client errors), and no internal implementation details (no stack traces, no DB error messages).

6. **Protect against injection and abuse** — Beyond type and format validation, check for: SQL/NoSQL injection patterns, path traversal sequences in filenames, excessively large inputs (payload size limits, string length limits), and mass assignment vulnerabilities (only accept explicitly allowed fields).

Constraints:
- Do not validate at the business logic layer what should be validated at the input layer — keep validation at the boundary.
- Do not return different error formats for different validation failures — standardize.
- Do not strip or silently coerce invalid input without telling the caller — reject and explain.
- Do not trust data from internal services without validation if they in turn receive external input.

## Inputs

- **Source code**: The endpoint, function, or handler to harden.
- **API contract**: (Optional) OpenAPI spec, documented schema, or expected input format.
- **Known abuse patterns**: (Optional) Any specific attacks or malformed inputs previously observed.

## Output

- **Validation gap report**: Table of each input field, current validation status, and what is missing.
- **Validation additions**: Code implementing the missing validations — shown as diffs or complete validated handler.
- **Error response examples**: What a validation failure response looks like after the fix.
- **Schema definition**: If a schema library is appropriate, the schema definition to add.
- **Security notes**: Any inputs that pose injection or abuse risk, with specific mitigations applied.

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

> **Usage note**: Run before deploying any new API endpoint and after `security-review` flags missing validation. Pair with `validation-schema-enforcement` to extend this to the full system.
