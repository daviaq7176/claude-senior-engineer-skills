# Skill: Frontend Performance

## Purpose

Reduce unnecessary re-renders, optimize bundle size, improve load times, and eliminate UI jank in frontend applications. Use this skill when a UI feels slow, when Core Web Vitals scores are poor, or when profiling reveals excessive rendering or large JavaScript payloads.

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

---

> **Usage note**: Always measure with real profiler data before and after. Perceived performance improvements (skeleton screens, optimistic UI) can be as valuable as actual performance gains for user experience.
