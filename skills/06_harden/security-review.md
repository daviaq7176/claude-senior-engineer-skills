# Skill: Security Review

## Purpose

Detect security vulnerabilities, insecure coding patterns, and missing security controls in application code. Use this skill before shipping a new feature, after a significant refactor, when working with authentication/authorization code, or when any user-controlled data is processed.

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

> **Usage note**: A security review is not a substitute for a threat model or penetration test. Use this skill for code-level review, and pair with `input-validation-fix` and `validation-schema-enforcement` to implement the remediations.
