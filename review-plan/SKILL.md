---
name: review-plan
description: Reviews a Claude Code implementation plan before acceptance. Invoke this to critically review any proposed plan against engineering best practices before approving it.
argument-hint: [optional focus area]
---

# Plan Review Mode

You are reviewing a plan that was just proposed. Do NOT make any code changes. Your job is to critically review the plan, surface issues, and get user sign-off before proceeding.

## Step 0 — Mandatory Sanity Check (NEVER skip this)

Before anything else, answer these questions:

1. **Does the plan actually solve the stated problem?** Re-read the original request and confirm the plan addresses it — not a related but different problem.
2. **What is the simplest possible solution?** Could this be done with fewer files, fewer abstractions, or a more direct approach?
3. **What does the plan NOT address?** List any edge cases, error states, or requirements that are missing or hand-waved.

## Step 1 — Architecture & Design Review

For each significant design decision in the plan:

- **State the decision** clearly.
- **List at least 2 alternatives** that were not chosen (or should have been considered).
- **Explain the concrete tradeoffs** (not vague pros/cons — specific: perf impact, maintenance burden, coupling introduced, testing difficulty).
- **Give an opinionated recommendation** based on the user's preferences below.
- **Flag if user input is needed** before proceeding.

## Step 2 — Code Quality Checklist

Evaluate the plan against these criteria:

- [ ] **DRY**: Does the plan introduce any duplication? Flag repetition aggressively — if two parts of the plan do similar things, call it out.
- [ ] **Minimal diff**: Can the goal be achieved with fewer new abstractions and fewer files touched? If the plan creates new files/classes/modules, justify each one.
- [ ] **Explicit over clever**: Does the plan favor straightforward, readable approaches? Flag any "clever" patterns that sacrifice clarity.
- [ ] **Edge cases**: Are error states, boundary conditions, race conditions, and failure modes addressed? List any that are missing. Err on the side of more coverage, not less.
- [ ] **Right-sized engineering**: Is the plan "engineered enough"? Flag both under-engineering (fragile, hacky, no error handling) AND over-engineering (premature abstraction, unnecessary complexity, speculative generality).
- [ ] **Security**: Any injection risks, auth gaps, or OWASP top-10 concerns?

## Step 3 — Test Strategy Diagram

Produce a test coverage diagram. This is MANDATORY — never skip it.

```
[Feature/Change being tested]
|
+-- Unit Tests
|   +-- [list specific functions/modules to unit test]
|   +-- [list edge cases each unit test should cover]
|
+-- Integration Tests
|   +-- [list integration points to test]
|   +-- [list failure scenarios]
|
+-- E2E / Manual Tests (if applicable)
    +-- [list user-facing flows to verify]
```

If the plan does not include a test strategy, **this is a blocker** — flag it prominently.

## Step 4 — Opinionated Recommendations

Summarize your top recommendations as a numbered list. For each:

1. **What to change** in the plan.
2. **Why** (the concrete risk or improvement).
3. **Severity**: `blocker` (must fix before implementing), `strong suggestion` (significantly better outcome), or `nice-to-have`.

## Step 5 — Decision Points

List any open questions or choices where user input is needed. Present them clearly:

> **Decision needed**: [question]
> - Option A: [description] — tradeoff: [...]
> - Option B: [description] — tradeoff: [...]
> - **Recommended**: [your pick and why]

## User's Engineering Preferences (use these to guide all recommendations)

- DRY is important — flag repetition aggressively.
- Well-tested code is non-negotiable; rather have too many tests than too few.
- Code should be "engineered enough" — not under-engineered (fragile, hacky) and not over-engineered (premature abstraction, unnecessary complexity).
- Err on the side of handling more edge cases, not fewer; thoughtfulness > speed.
- Bias toward explicit over clever.
- Minimal diff: achieve the goal with the fewest new abstractions and files touched.
- System thinking: avoid local optimization, think in bigger picture, each new feature should cleanly fit the overall architecture.

## Priority Hierarchy (if running low on context)

Step 0 > Test diagram > Opinionated recommendations > Everything else. **Never skip Step 0 or the test diagram.**

---

If `$ARGUMENTS` is provided, focus the review especially on: $ARGUMENTS
