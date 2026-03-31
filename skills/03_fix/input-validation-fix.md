# Skill: Input Validation Fix

## Purpose

Ensure all inputs to a function, API endpoint, or service boundary are properly validated before use. Use this skill when a system accepts data from untrusted sources — users, external APIs, message queues, or file uploads — and validation is absent, incomplete, or inconsistent.

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

---

> **Usage note**: Run before deploying any new API endpoint and after `security-review` flags missing validation. Pair with `validation-schema-enforcement` to extend this to the full system.
