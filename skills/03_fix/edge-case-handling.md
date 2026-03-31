# Skill: Edge Case Handling

## Purpose

Identify and add handling for edge cases and boundary conditions that the current code does not account for. Use this skill after writing or reviewing a function to harden it against unexpected inputs, extreme values, and real-world conditions the happy path doesn't cover.

## Instructions

You are acting as a senior software engineer systematically hardening code against edge cases. Follow these steps precisely:

1. **Enumerate the input space** — For each input parameter, list the range of possible values including: null/undefined/None, empty string/array/object, zero and negative numbers, very large numbers, maximum allowed values, whitespace-only strings, special characters, non-ASCII characters, and values of the wrong type.

2. **Check boundary conditions** — For any operation involving ranges, indices, or thresholds, verify behavior at: the exact boundary, one below the boundary, one above the boundary, and the maximum representable value. Off-by-one errors live here.

3. **Identify missing state checks** — Find calls to methods or operations that assume a certain state (object is initialized, list is non-empty, connection is open, user is authenticated). For each, verify there is a guard before the assumption is relied on.

4. **Test the empty/zero case** — Verify the code handles: empty collections in loops, zero divisors in divisions, empty results from database queries, zero-length files, and empty responses from external APIs.

5. **Consider concurrent and repeated invocations** — What happens if the function is called twice with the same input? What if it's called before a prior call completes? What if it's called while the system is shutting down?

6. **Write the missing cases** — For each unhandled edge case found, add: an explicit guard or validation, a meaningful error or return value, and a comment explaining the edge case if it is non-obvious.

Constraints:
- Do not add defensive checks that are already guaranteed by the caller — over-defensive code becomes noise.
- Do not silently swallow errors — every edge case that is caught must either be corrected, returned, or re-raised with context.
- Do not change the happy-path behavior while adding edge case handling.
- Prefer early returns over nested conditionals for guard clauses.

## Inputs

- **Source code**: The function or module to harden.
- **Known failure cases**: (Optional) Specific edge cases that have already caused problems.
- **Input contracts**: (Optional) What callers are documented or expected to pass.

## Output

- **Edge case inventory**: Comprehensive list of edge cases found, categorized by type (null, boundary, state, concurrent).
- **Current behavior**: What the code does today for each unhandled case (crash, wrong result, silent pass-through).
- **Code additions**: The specific guard clauses, validations, and error returns to add — shown as diffs or annotated snippets.
- **Test cases**: One test per edge case, covering the input and expected behavior after the fix.
- **Remaining risks**: Edge cases that were identified but not handled, with rationale for deferring.

---

> **Usage note**: Run this skill after `patch-critical-bug` to harden beyond the immediate fix, and before `automated-test-generation` to ensure the tests cover all the important cases.
