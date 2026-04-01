# Skill: [Skill Name]

## Purpose

[One to three sentences. Be specific. What problem does this skill solve? What kind of code or situation does it apply to? Avoid generic descriptions — a reader should immediately know if this skill is right for their situation.]

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

You are acting as a senior software engineer performing [task type]. Follow these steps precisely:

1. **[First step]** — [What to do and why. Include constraints where relevant.]
2. **[Second step]** — [Be concrete. Reference specific things to look for or produce.]
3. **[Third step]** — [...]

Constraints:
- Do not [common mistake or anti-pattern to avoid].
- Preserve [what must not change — behavior, interfaces, contracts].
- Flag [what to surface rather than silently fix].
- Prefer [what kind of solution to favor — minimal, idiomatic, reversible, etc.].

When in doubt, [decision rule — e.g., "prefer caution over cleverness", "ask before assuming intent"].

## Inputs

- **[Input 1]**: [Description — e.g., "The source file or module under analysis"]
- **[Input 2]**: [Description — e.g., "Error logs, stack traces, or test output"]
- **[Input 3]**: [Description — e.g., "Additional context: language, framework, constraints"]

## Output

[Describe the exact shape of what should be produced. Use bullet points or a structured format.]

- **[Section 1]**: [e.g., "Summary: 2–4 sentence plain-English description of findings"]
- **[Section 2]**: [e.g., "Findings: Bulleted list of specific issues with file/line references"]
- **[Section 3]**: [e.g., "Recommendations: Ordered list of actions, most critical first"]
- **[Section 4 if applicable]**: [e.g., "Code changes: Diffs or annotated snippets"]

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

> **Usage note**: [Optional tip on when to use this skill, how to combine it with others, or what to watch out for.]
