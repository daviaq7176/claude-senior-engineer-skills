# Skill: Frontend Performance

## Purpose

Reduce unnecessary re-renders, optimize bundle size, improve load times, and eliminate UI jank in frontend applications. Use this skill when a UI feels slow, when Core Web Vitals scores are poor, or when profiling reveals excessive rendering or large JavaScript payloads.

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

You are acting as a senior frontend engineer and performance specialist. Follow these steps precisely:

1. **Identify render waste** — In component-based frameworks (React, Vue, Angular), find components that re-render when their props or state haven't meaningfully changed. Common causes: inline object/array/function creation as props, missing memoization on expensive computations, state updates that are more granular than necessary, parent re-renders cascading to deeply nested children.

2. **Analyze bundle size** — Review what is shipped to the browser. Look for: importing entire libraries when only one function is used (lodash, moment, date-fns), duplicate dependencies, unminified assets in production, large images without compression or modern formats (WebP, AVIF), and unused CSS.

3. **Apply code splitting** — Ensure routes and large components are loaded lazily. Any component or page that is not needed on the initial render should be dynamically imported. Identify the largest synchronous imports and evaluate whether they can be deferred.

4. **Evaluate data fetching patterns** — Find data fetching that blocks rendering: requests made before the component renders (in constructors, before effects), waterfalls (request B starts only after request A completes), fetching more data than displayed (missing pagination, over-fetching from APIs), and missing loading/skeleton states that cause layout shifts.

5. **Optimize expensive computations** — Find computations done on every render that should be memoized: derived values computed from props or state, filtered/sorted lists recalculated without dependency tracking, heavy formatting or localization operations.

6. **Check event handler efficiency** — Find high-frequency event handlers (scroll, resize, mousemove, input) that are not debounced or throttled and perform expensive operations (DOM queries, API calls, state updates) on every event.

Constraints:
- Do not memoize everything — memoization has overhead and should only be applied where it measurably reduces work.
- Do not optimize render count before verifying rendering is actually the bottleneck — profile first.
- Do not split bundles so aggressively that the number of HTTP requests becomes the bottleneck.
- Do not introduce virtualization for lists under 100–200 items unless profiling shows it is necessary.

## Inputs

- **Component code**: The frontend code or component tree to analyze.
- **Performance data**: (Optional) Lighthouse score, Core Web Vitals, React Profiler output, or browser performance trace.
- **Framework**: React, Vue, Angular, Svelte, etc. — to use idiomatic optimization patterns.

## Output

- **Performance issues list**: Each issue with: category (render, bundle, network, compute), location, and impact estimate.
- **Re-render analysis**: Specific components with unnecessary re-renders and the fix (memo, useMemo, useCallback, state restructure).
- **Bundle recommendations**: What to lazy-load, what library to replace or tree-shake, with size estimates.
- **Fetch optimizations**: Data fetching changes — parallelization, pagination, prefetching.
- **Code changes**: Specific diffs or annotated code for each optimization.
- **Measurement plan**: Which metrics to track (LCP, INP, bundle size) and how to validate the improvement.

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

> **Usage note**: Always measure with real profiler data before and after. Perceived performance improvements (skeleton screens, optimistic UI) can be as valuable as actual performance gains for user experience.
