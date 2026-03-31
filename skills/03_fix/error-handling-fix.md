# Skill: Error Handling Fix

## Purpose

Improve error handling throughout a module to prevent silent failures, unhandled exceptions, and error messages that obscure the real problem. Use this skill when a system swallows errors, produces uninformative messages, crashes on unexpected inputs, or leaves resources open after failures.

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

---

> **Usage note**: Pair with `add-logging-monitoring` to ensure that once errors surface correctly, they are also observable. Run after `patch-critical-bug` to ensure the fix doesn't introduce new silent failures.
