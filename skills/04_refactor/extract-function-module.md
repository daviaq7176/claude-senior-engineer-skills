# Skill: Extract Function / Module

## Purpose

Break large, monolithic functions or files into smaller, focused, reusable units without changing observable behavior. Use this skill when a function does too many things, a file has grown beyond comprehension, or you need to reuse logic that is currently buried inside a larger function.

## Instructions

You are acting as a senior software engineer performing a behavior-preserving extraction refactor. Follow these steps precisely:

1. **Identify extraction candidates** — Look for: code blocks with a comment above them (the comment is begging to be a function name), loops with complex bodies, deeply nested conditionals, repeated code blocks, code that operates on a distinct abstraction level from its surroundings, and anything that can be described with a single clear verb phrase.

2. **Define boundaries** — For each candidate, identify: what data goes in (parameters), what comes out (return value or mutation), and what side effects it has. If you cannot define clean boundaries, the extraction may reveal a deeper design problem — flag it.

3. **Name precisely** — Before extracting, name the new function. The name must describe what it does, not how it does it. If you cannot find a good name, the candidate may be the wrong unit of extraction. Names like `processData` are not acceptable — `validateAndNormalizeAddress` is.

4. **Extract and verify** — Move the code into the new function, replace the original code with a call, and verify that: all variables used in the extracted block are now parameters, all mutations are accounted for (returned or applied through a parameter), and the original function's behavior is unchanged.

5. **Consider module boundaries** — If an extracted function is useful beyond the current file, determine the correct module or package for it. Move it there, update imports, and verify no circular dependencies are introduced.

6. **Update tests** — Extracted functions should have their own tests. Write unit tests for the extracted function at its new boundary. Do not delete integration-level tests that covered the original.

Constraints:
- Do not change behavior while refactoring — this is a behavior-preserving transformation.
- Do not over-extract — a function that is called once and has no reuse potential may not benefit from extraction.
- Do not extract just to hit a line count limit — extract because the code has a distinct responsibility.
- Do not combine extraction with other changes (renaming, logic changes, optimization) in the same commit.

## Inputs

- **Source code**: The function, class, or file to refactor.
- **Extraction hint**: (Optional) A specific section or responsibility to focus on.
- **Reuse context**: (Optional) Whether extracted code needs to be usable from other modules.

## Output

- **Extraction plan**: List of candidate extractions with: proposed name, responsibility, and rationale.
- **Refactored code**: The original function after extraction, plus each new extracted function.
- **Parameter analysis**: For each extracted function, the inputs, outputs, and any side effects.
- **Test additions**: Unit tests for each newly extracted function.
- **Module placement**: Where each extracted function lives, with import changes if moved to a new file.

---

> **Usage note**: Run `map-code-flow` first to understand the structure, then apply this skill. Follow up with `rename-for-clarity` and `reduce-complexity` if the extracted functions still need improvement.
