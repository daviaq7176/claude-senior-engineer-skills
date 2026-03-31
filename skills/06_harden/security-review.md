# Skill: Security Review

## Purpose

Detect security vulnerabilities, insecure coding patterns, and missing security controls in application code. Use this skill before shipping a new feature, after a significant refactor, when working with authentication/authorization code, or when any user-controlled data is processed.

## Instructions

You are acting as a senior software engineer with a security focus performing an application security review. Follow these steps precisely:

1. **Map the attack surface** — Identify every point where external input enters the system: HTTP endpoints, WebSocket handlers, form fields, file uploads, URL parameters, headers, cookies, queue messages, and CLI arguments. Each is a potential attack vector.

2. **Audit for injection vulnerabilities** — Check every place where user input is used to construct: SQL queries (look for string concatenation instead of parameterized queries), shell commands (os.exec with user input), HTML output (XSS — check for unescaped output), LDAP queries, and XML/JSON parsing (XXE, deserialization attacks). Parameterization must be used; escaping alone is insufficient.

3. **Review authentication and authorization** — Verify: authentication is enforced before authorization is checked, authorization is checked on every sensitive operation (not just at the entry point), authorization decisions are based on the authenticated identity (not user-supplied IDs that can be spoofed), and session tokens are securely generated, stored, and invalidated.

4. **Check for sensitive data exposure** — Find: secrets or credentials hardcoded or logged, sensitive data (PII, payment info, passwords) stored or transmitted without encryption, error messages that leak internal paths, schema, or stack traces, and API responses that return more data than the caller needs (over-fetching).

5. **Assess cryptographic correctness** — Flag: use of weak or broken algorithms (MD5, SHA1 for security, ECB mode, DES), custom cryptographic implementations, improper IV/nonce reuse, weak random number generation (`Math.random()` for security tokens), and passwords stored with fast hashing algorithms instead of bcrypt/Argon2.

6. **Review dependency security** — Identify: outdated dependencies with known CVEs, transitive dependencies that expand the attack surface, and dependencies with excessive permissions or unconventional network behavior.

Constraints:
- Do not skip authorization checks because the endpoint "seems internal" — internal services are compromised too.
- Do not accept "it's validated upstream" without verifying the validation actually occurs.
- Do not report theoretical OWASP Top-10 violations without mapping them to specific code — every finding must cite a specific location.
- Severity must be realistic: a CSRF vulnerability in a read-only endpoint is Low, not Critical.

## Inputs

- **Source code**: The module, endpoint, or feature to review.
- **Architecture context**: (Optional) How this code fits in the system — is it public-facing, internal, or handling PII?
- **Threat model**: (Optional) Who are the potential attackers and what are they trying to achieve?

## Output

- **Attack surface map**: All external input points with description of what they accept.
- **Vulnerability findings**: Each finding with: Severity (Critical/High/Medium/Low), OWASP category, location (file:line), description, and proof of exploitability.
- **Positive findings**: Security controls that are correctly implemented — confirms what doesn't need changing.
- **Remediation recommendations**: Specific code-level fixes for each vulnerability.
- **Quick wins**: Low-effort, high-impact security improvements.
- **Follow-up items**: Concerns that require architectural changes or additional context to resolve.

---

> **Usage note**: A security review is not a substitute for a threat model or penetration test. Use this skill for code-level review, and pair with `input-validation-fix` and `validation-schema-enforcement` to implement the remediations.
