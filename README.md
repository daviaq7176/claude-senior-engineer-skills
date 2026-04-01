# Claude Senior Engineer Skills

> **From Bug → Clean → Scalable → Production**

A curated collection of 30 Claude-style skills that mirror how senior engineers *actually* think about code — not just writing it, but understanding, debugging, fixing, refactoring, optimizing, hardening, and reviewing it.

These skills are designed to be used with [Claude Code](https://claude.ai/code) or any Claude-powered AI workflow. Each skill is a precise, opinionated instruction set that transforms Claude into a domain-specific expert for a given engineering task.

---

## 🧠 The Philosophy

Most AI coding prompts are shallow. They say "fix this bug" or "refactor this function" without the depth a senior engineer brings to the table.

This repository encodes the **mental models, constraints, and judgment** of a seasoned engineer into reusable, composable skill files. The result: AI assistance that doesn't just produce code — it produces *correct*, *safe*, *maintainable* code backed by real engineering thinking.

---

## 🚀 What's New: Advanced AI Techniques

Every skill now includes **advanced prompting techniques** that dramatically improve Claude's reasoning quality:

| Technique | What It Does |
|-----------|--------------|
| 🧠 **Reasoning Protocol** | Forces explicit step-by-step thinking: Observe → Recognize Patterns → Form Hypothesis → Gather Evidence → Conclude |
| ✅ **Self-Verification Checklist** | Built-in quality gate: evidence grounded? assumptions stated? edge cases covered? |
| 🎯 **Confidence Assessment** | Tags findings as 🟢 High / 🟡 Medium / 🔴 Low confidence with required actions |
| ⚔️ **Adversarial Self-Review** | "What would a skeptical senior engineer challenge?" — addresses strongest objection |

### Why This Matters

Traditional prompts produce **answers**. These skills produce **auditable reasoning**:

```
❌ Old: "The bug is in line 42"
✅ New: "Based on [evidence], I hypothesize [X]. This is supported by [Y] but
        contradicted by [Z]. Confidence: Medium. Verify by [action]."
```

---

## 🔄 The Pipeline

| Stage | Folder | Skills | Purpose |
|-------|--------|--------|---------|
| 🔍 **Understand** | `01_understand/` | 4 | Map, analyze, and explain existing code |
| 🐛 **Debug** | `02_debug/` | 4 | Reproduce, trace, and diagnose issues |
| 🔧 **Fix** | `03_fix/` | 4 | Apply targeted, safe, minimal patches |
| ♻️ **Refactor** | `04_refactor/` | 4 | Improve structure without changing behavior |
| ⚡ **Optimize** | `05_optimize/` | 4 | Eliminate bottlenecks and improve efficiency |
| 🛡️ **Harden** | `06_harden/` | 4 | Secure, monitor, and make resilient |
| 🔬 **Review** | `07_review/` | 6 | Architect, test, and future-proof |

---

## 📁 Repository Structure

```
senior-engineer-skills/
├── README.md
├── LICENSE
├── templates/
│   └── skill-template.md
└── skills/
    ├── 01_understand/
    │   ├── map-code-flow.md
    │   ├── identify-risky-areas.md
    │   ├── dependency-analysis.md
    │   └── explain-business-logic.md
    ├── 02_debug/
    │   ├── reproduce-bug-from-logs.md
    │   ├── trace-execution-path.md
    │   ├── detect-race-conditions.md
    │   └── analyze-performance-bottlenecks.md
    ├── 03_fix/
    │   ├── patch-critical-bug.md
    │   ├── edge-case-handling.md
    │   ├── input-validation-fix.md
    │   └── error-handling-fix.md
    ├── 04_refactor/
    │   ├── extract-function-module.md
    │   ├── reduce-complexity.md
    │   ├── rename-for-clarity.md
    │   └── introduce-design-patterns.md
    ├── 05_optimize/
    │   ├── optimize-queries.md
    │   ├── frontend-performance.md
    │   ├── memory-cpu-efficiency.md
    │   └── caching-strategies.md
    ├── 06_harden/
    │   ├── security-review.md
    │   ├── add-logging-monitoring.md
    │   ├── validation-schema-enforcement.md
    │   └── fail-safe-retry-logic.md
    └── 07_review/
        ├── architecture-review.md
        ├── automated-test-generation.md
        ├── refactor-legacy-code-incrementally.md
        ├── tradeoff-analysis.md
        ├── code-style-consistency.md
        └── future-proofing-analysis.md
```

---

## 🚀 How to Use

### With Claude Code

Copy any skill file's content into your Claude Code session as a CLAUDE.md skill, or reference it directly:

```bash
cat skills/02_debug/trace-execution-path.md >> CLAUDE.md
```

### As a System Prompt

Use the skill's **Instructions** section as a system prompt when starting a new Claude conversation focused on that task.

### In a Pipeline

Chain skills together for end-to-end workflows:

```
1. map-code-flow        → understand the system
2. identify-risky-areas → find where to focus
3. reproduce-bug        → confirm the problem
4. trace-execution-path → understand the root cause
5. patch-critical-bug   → apply the fix
6. security-review      → verify no new vulnerabilities
7. automated-test-generation → lock in the fix
```

---

## 💡 Example Usage

### Scenario: You inherited a broken payment service

```
Step 1 → skills/01_understand/explain-business-logic.md
  "What does this payment flow actually do?"

Step 2 → skills/02_debug/reproduce-bug-from-logs.md
  "Here are the Stripe webhook logs from the failed transactions..."

Step 3 → skills/02_debug/detect-race-conditions.md
  "There's a double-charge happening under load..."

Step 4 → skills/03_fix/patch-critical-bug.md
  "Apply the idempotency fix without touching unrelated code."

Step 5 → skills/06_harden/fail-safe-retry-logic.md
  "Add retry logic with backoff for transient Stripe failures."

Step 6 → skills/07_review/automated-test-generation.md
  "Generate tests covering the race condition and edge cases."
```

---

## ✨ Why This Is Different

Each skill encodes:
- **Constraints** — what NOT to do (no unnecessary rewrites, preserve behavior, minimal blast radius)
- **Senior-level framing** — how to think about the problem, not just how to execute
- **Structured outputs** — so you always know what you're getting back
- **Real-world focus** — edge cases, concurrency, production failures, legacy code
- **Explicit reasoning** — show your work, not just conclusions
- **Built-in verification** — self-check before delivering results
- **Calibrated confidence** — know when to trust the output and when to verify

---

## 📋 Skill Anatomy

Every skill follows a consistent structure:

```markdown
# Skill: [Name]

## Purpose
What problem this solves and when to use it

## Reasoning Protocol          ← NEW: Structured thinking before output
<reasoning>
1. Observation → 2. Pattern Recognition → 3. Hypothesis → 4. Evidence → 5. Conclusion
</reasoning>

## Instructions
Step-by-step execution guide with constraints

## Inputs
What the skill needs to work

## Output
Exact shape of what you'll get back

## Self-Verification Checklist  ← NEW: Quality gate before delivery
- [ ] Evidence grounded
- [ ] No speculation as fact
- [ ] Edge cases considered
- [ ] Assumptions explicit

## Confidence Assessment        ← NEW: Know when to trust output
🟢 High (90%+) | 🟡 Medium (60-90%) | 🔴 Low (<60%)

## Adversarial Self-Review      ← NEW: Challenge your own analysis
What did I miss? What would a skeptic challenge?
```

---

## 🤝 Contributing

1. Fork the repository
2. Copy `templates/skill-template.md`
3. Fill in all sections completely — no vague instructions
4. **Include all advanced sections**: Reasoning Protocol, Self-Verification, Confidence Assessment, Adversarial Review
5. Place it in the appropriate stage folder
6. Open a PR with: skill name, purpose, and the problem it solves

**Quality bar:** Your skill should read like it was written by a principal engineer who has felt the pain this skill addresses. Every claim must be traceable to evidence.

---

## 📄 License

MIT — use freely, attribute if you share.
