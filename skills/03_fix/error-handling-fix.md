# Skill: Error Handling Fix

## Purpose

Improve error handling throughout a module to prevent silent failures, unhandled exceptions, and error messages that obscure the real problem. Use this skill when a system swallows errors, produces uninformative messages, crashes on unexpected inputs, or leaves resources open after failures.

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

You are acting as a senior software engineer auditing and fixing error handling. Follow these steps precisely:

1. **Find silent failures** — Search for every empty catch block, caught exception that is only logged with no action taken, ignored Promise rejections, unchecked return codes, and results that are used without checking for error states. These are the most dangerous — the system continues in a broken state without anyone knowing.

2. **Audit catch scope** — Find `catch` blocks that catch overly broad exceptions (e.g., catching `Exception` instead of `IOException`). Broad catches mask programming errors as operational errors. Each catch should handle a specific, expected failure mode.

3. **Check resource cleanup** — Verify that all resources (file handles, DB connections, network sockets, locks) are released even when errors occur. In languages without `using`/`with`/`try-with-resources`, verify `finally` blocks are used consistently.

4. **Evaluate error propagation** — Determine whether errors are being propagated correctly up the call stack. An error caught deep in a utility function that returns `null` instead of propagating loses all context. Errors should bubble up with context added at each layer, not be swallowed at each layer.

5. **Improve error messages** — Find errors that produce messages like "something went wrong" or "unexpected error". Replace with messages that include: what operation failed, what the system was trying to do, what input was involved (without PII), and what the caller can do about it.

6. **Add structured error types** — Where errors are just strings or generic exceptions, introduce typed errors that can be handled specifically by callers. A `DatabaseConnectionError` is actionable; a `RuntimeException` is not.

Constraints:
- Do not catch errors unless you can handle them meaningfully at that layer — prefer propagation over suppression.
- Do not log and re-throw unless the log adds context the stack trace doesn't already provide.
- Do not include sensitive data (passwords, tokens, PII) in error messages or logs.
- Do not convert exceptions to error codes or null without documenting the contract — callers must know what to check.
- Preserve the original exception as the cause/inner exception when wrapping.

## Inputs

- **Source code**: The module or function to audit.
- **Error patterns**: (Optional) Known error types or failure modes that have caused incidents.
- **Target environment**: (Optional) Language and framework, to suggest idiomatic error handling patterns.

## Output

- **Silent failure inventory**: List of every place errors are swallowed or ignored, with severity.
- **Broad catch list**: Every overly broad catch with the specific exception types it should catch instead.
- **Resource leak risks**: Any code paths where resources aren't cleaned up on failure.
- **Code fixes**: Diffs or annotated snippets for each finding.
- **Error message rewrites**: Before/after for each improved error message.
- **Error type definitions**: New typed error classes or enums to introduce, if applicable.

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

> **Usage note**: Pair with `add-logging-monitoring` to ensure that once errors surface correctly, they are also observable. Run after `patch-critical-bug` to ensure the fix doesn't introduce new silent failures.
