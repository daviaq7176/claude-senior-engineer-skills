# Skill: Identify Risky Areas

## Purpose

Detect parts of a codebase that are fragile, overly complex, poorly tested, or likely to cause production incidents. Use this skill before making changes to a module, planning a refactor, or conducting a pre-release review. The goal is not to fix anything — it is to produce an honest risk map.

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

You are acting as a senior software engineer performing a risk assessment of the provided code. Follow these steps precisely:

1. **Scan for complexity hotspots** — Identify functions or classes with high cyclomatic complexity (many branches, deep nesting, long method bodies). Flag anything that requires significant mental effort to follow.

2. **Look for fragile assumptions** — Find places where the code assumes: input is always valid, external services always respond, state is always consistent, or ordering is always guaranteed. These assumptions are where bugs hide.

3. **Detect implicit coupling** — Identify code that is secretly dependent on global state, environment variables, execution order, or shared mutable data. These dependencies aren't in the function signature and are easy to break.

4. **Find error-handling gaps** — Note every place where errors are silently swallowed, exceptions are caught too broadly, or failure modes aren't considered. Especially in I/O paths, DB interactions, and external API calls.

5. **Assess test coverage signals** — Even without running tests, identify code that is difficult to test in isolation (tight coupling, no dependency injection, side effects in constructors, etc.). These areas are likely undertested.

6. **Flag technical debt markers** — Look for TODO/FIXME/HACK comments, disabled assertions, workarounds with no explanation, and code that contradicts its own comments.

Constraints:
- Do not propose fixes in this skill — risk identification only.
- Rank findings by severity: Critical (likely to cause production failure), High (likely to cause bugs under load or edge cases), Medium (maintainability risk), Low (minor concern).
- Do not flag style issues as risks unless they create ambiguity that leads to bugs.
- Be specific: cite file, function name, and line range for every finding.

## Inputs

- **Source code**: The module or codebase to assess.
- **Context**: (Optional) Known pain points, recent incidents, or areas of concern to prioritize.
- **Change intent**: (Optional) What change is planned, to focus the risk assessment on relevant areas.

## Output

- **Risk summary**: 2–3 sentence executive summary of the overall risk level and most pressing concerns.
- **Findings table**: Each finding with: Severity | Location (file:line) | Description | Why it's risky.
- **Top 3 critical areas**: Expanded explanation of the three highest-risk findings with concrete failure scenarios.
- **Testing gaps**: List of areas most likely to be undertested, with reasoning.
- **Recommended focus**: Which areas to address first if time is limited.

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

> **Usage note**: Combine with `map-code-flow` for full context, and hand off to `patch-critical-bug` or `refactor-legacy-code-incrementally` for the follow-on fix work.
