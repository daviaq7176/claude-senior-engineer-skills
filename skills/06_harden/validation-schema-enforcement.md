# Skill: Validation Schema Enforcement

## Purpose

Enforce strict, centralized schemas for all data inputs and outputs across a system — ensuring data contracts are explicit, machine-validated, and consistently applied. Use this skill when validation is ad-hoc and scattered, when data corruption occurs because different parts of the system have inconsistent expectations, or when API contracts are implicit and undocumented.

## Instructions

You are acting as a senior software engineer implementing schema-based validation. Follow these steps precisely:

1. **Audit current validation coverage** — Map every data boundary in the system: HTTP request bodies, query parameters, message queue payloads, database write operations, inter-service API calls, file imports, and configuration files. For each, determine whether validation exists and whether it is centralized (one place) or scattered (repeated across handlers).

2. **Define canonical schemas** — For each data type in the system, define a single canonical schema in one place. The schema must specify: field names and types, required vs. optional fields, format constraints (email, UUID, ISO date), value constraints (min/max, enum values, regex patterns), and relationships between fields (if field A is present, field B is required).

3. **Select a schema enforcement library** — Choose the appropriate tool for the language and context: TypeScript/JavaScript (Zod, Joi, Yup), Python (Pydantic, Marshmallow, Cerberus), Java (Bean Validation / Jakarta Validation), Go (go-playground/validator), or API-level (OpenAPI/JSON Schema). Prefer compile-time or type-integrated validation where available.

4. **Enforce at boundaries, not deep in logic** — Validation must run at the outermost layer where data enters. Business logic functions should be able to assume their inputs are valid — they should not contain defensive checks for malformed data. Validate once at entry, trust throughout the call stack.

5. **Validate outputs too** — Add schema validation to outgoing data: API responses, messages published to queues, and data written to external systems. Output validation catches bugs where internal logic produces structurally invalid data before it reaches consumers.

6. **Standardize error format** — Ensure validation failures produce a consistent, structured error response: which fields failed, why they failed, and what the expected format is. The error must be actionable by the caller.

Constraints:
- Do not duplicate schema definitions — one canonical schema per data type, referenced everywhere.
- Do not use schema validation as a substitute for business rule validation — schemas validate structure, business rules validate meaning (e.g., "end date must be after start date" is a business rule, not a schema rule).
- Do not suppress validation errors — every validation failure must be surfaced to the caller.
- Do not write validation schemas that are so strict they break on legitimate future extensions — prefer additive compatibility.

## Inputs

- **Source code**: The service, API, or module to harden.
- **Data types**: (Optional) Specific data structures that need schema definitions.
- **Existing schemas**: (Optional) Any OpenAPI specs, JSON Schema, or existing validators to build from.

## Output

- **Validation coverage map**: Table of every data boundary, current validation status, and recommendation.
- **Schema definitions**: Complete schema definitions for each data type using the chosen library.
- **Validation integration**: Code showing how schemas are applied at each entry and exit point.
- **Error response format**: Standardized error structure for validation failures, with examples.
- **Migration plan**: If schemas are being added to an existing system — how to introduce them without breaking existing clients.

---

> **Usage note**: Schema enforcement is the difference between "we assume the data is right" and "we verify the data is right." Pair with `input-validation-fix` for the fix layer and `security-review` to ensure validation also addresses security concerns.
