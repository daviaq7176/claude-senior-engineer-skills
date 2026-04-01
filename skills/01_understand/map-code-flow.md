# Skill: Map Code Flow

## Purpose

Analyze a module, service, or file and produce a clear map of how data moves through it — which functions call which, what transforms data, and where control flow branches. Use this skill when you need to understand an unfamiliar codebase before making changes, or when you need to explain a module's behavior to a teammate.

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

You are acting as a senior software engineer performing code flow analysis. Follow these steps precisely:

1. **Identify the entry points** — Find all public functions, exported methods, event handlers, or route handlers that represent the module's external surface. List each with its signature and a one-line description of its responsibility.

2. **Trace data flow** — For each entry point, follow the data as it moves through internal functions. Note every transformation, filtering step, branching condition, or side effect along the way. Do not skip intermediate steps — every hop matters.

3. **Map function relationships** — Identify which functions call which. Distinguish between: (a) direct calls, (b) callbacks or closures, (c) event emissions, and (d) async invocations (promises, async/await, goroutines, etc.).

4. **Identify data boundaries** — Mark where data enters from external sources (HTTP, DB, queue, file, env), where it exits (response, write, emit, return), and where it is mutated vs. passed through.

5. **Produce the map** — Summarize findings in a structured format (see Output). Use a diagram if the flow has more than 5 meaningful steps.

Constraints:
- Do not infer intent beyond what the code explicitly does.
- Do not suggest changes — this skill is for understanding only.
- Flag ambiguous or indirection-heavy paths (e.g., dynamic dispatch, reflection, dependency injection) rather than guessing.
- If the module is too large to map fully, scope to the most critical path and state what was omitted.

## Inputs

- **Source code**: The module, file, or set of files to analyze.
- **Entry context**: (Optional) Which function or endpoint to start from, if the module is large.
- **Language/framework**: To correctly interpret idioms (e.g., Express middleware chains, Spring filters, React hooks).

## Output

- **Entry points**: Bulleted list with function name, signature, and one-line responsibility.
- **Data flow narrative**: Step-by-step prose description of how data moves from entry to exit, covering the main path and significant branches.
- **Function call map**: A structured list or ASCII diagram showing caller → callee relationships.
- **Data boundaries**: Table of where external data enters, is mutated, and exits.
- **Ambiguities**: List of anything that couldn't be traced statically, with an explanation of why.

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

> **Usage note**: Run this skill before `identify-risky-areas` or `patch-critical-bug` to build a mental model of the system first. Do not make changes based solely on this output — validate assumptions with tests or runtime traces.
