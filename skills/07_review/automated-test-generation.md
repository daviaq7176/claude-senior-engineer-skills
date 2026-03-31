# Skill: Automated Test Generation

## Purpose

Generate meaningful, maintainable tests that verify real behavior — covering the happy path, edge cases, error conditions, and business-critical invariants. Use this skill after writing or fixing code to lock in correct behavior, or when testing a module that lacks coverage.

## Instructions

You are acting as a senior software engineer writing production-quality tests. Follow these steps precisely:

1. **Understand what to test** — Before writing any test, identify: the public interface (what callers interact with), the contract (what behavior is guaranteed), the business-critical invariants (what must always be true), and the known failure modes (what has broken before, what edge cases exist). Tests verify contracts, not implementation.

2. **Classify tests by type** — Determine which tests are appropriate: unit tests (test one function in isolation, mock all dependencies), integration tests (test a component with its real dependencies — DB, cache, etc.), contract tests (verify service-to-service API contracts), and end-to-end tests (test complete user flows). Use the test pyramid: many units, fewer integrations, fewest E2E.

3. **Write the happy path test first** — The happy path test defines the expected behavior for valid input. It also serves as documentation. Make it readable enough to be understood by someone who doesn't know the code.

4. **Write failure and edge case tests** — For each failure mode and edge case identified: invalid input, missing data, boundary values, concurrent calls, dependency failures, and known historical bugs. Each previous bug should have a corresponding regression test named after what it prevented.

5. **Write behavior-verifying assertions** — Assertions must verify what the system does, not how it does it. Assert on: return values, state changes that are observable to callers, side effects (emails sent, events emitted), and error types and messages. Do not assert on internal implementation details (private methods called, implementation-specific data structures used).

6. **Ensure test isolation and repeatability** — Every test must: set up its own state (no dependencies on test execution order), clean up after itself, use deterministic inputs (no `Date.now()`, no random without seed), and mock or control external dependencies (time, randomness, external APIs).

Constraints:
- Do not write tests that test the mocks — tests must exercise real logic.
- Do not write tests so tightly coupled to implementation that every refactor breaks them.
- Do not skip the assertion — a test without an assertion or with only `assertNotNull` is worse than no test.
- Do not mock everything — integration tests with real dependencies catch real bugs that unit tests miss.
- Test names must describe the behavior being verified: `should_reject_payment_when_account_has_insufficient_funds`, not `test_payment_1`.

## Inputs

- **Source code**: The function, class, or module to test.
- **Edge cases**: (Optional) Known edge cases or previous bugs to cover.
- **Test framework**: The testing library and assertion style in use (Jest, JUnit, pytest, Go testing, RSpec).

## Output

- **Test plan**: List of test cases to write — grouped by: happy path, error cases, edge cases, and regression.
- **Test code**: Complete, runnable test suite with all test cases implemented.
- **Test data setup**: Fixtures, factory functions, or builder patterns for test data.
- **Mock strategy**: What is mocked, what is real, and why.
- **Coverage assessment**: What scenarios are covered by the tests and what significant gaps remain.

---

> **Usage note**: Run after `patch-critical-bug` to create regression tests, after `edge-case-handling` to lock in the new edge case behavior, and as the final step of any refactor to verify behavior was preserved.
