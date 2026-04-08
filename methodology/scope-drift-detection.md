# Scope Drift Detection

**Post-implementation audit that catches both scope creep and missing requirements.**

---

## The Problem

AI agents have two opposing failure modes:

1. **Scope creep**: The agent does more than asked -- adds features, refactors unrelated code, "improves" things that weren't in the plan
2. **Missing requirements**: The agent does less than asked -- skips edge cases, leaves TODOs, implements the happy path only

Both are invisible until someone explicitly checks. Scope Drift Detection makes the check mechanical.

---

## The Method

After completing a plan or PR, compare the original specification against the actual changes:

### Step 1: Gather the Diff

```bash
git diff --stat [base-branch]...HEAD
```

This shows every file that changed and how much.

### Step 2: Cross-Reference Against the Plan

For each file in the diff:

1. **Is this file mentioned in the plan?**
   - Yes -> Expected change. Verify it matches the plan's intent.
   - No -> **Potential scope creep.** Why was this file touched?

For each step in the plan:

2. **Is there a corresponding change in the diff?**
   - Yes -> Requirement addressed. Verify completeness.
   - No -> **Missing requirement.** Why wasn't this step implemented?

### Step 3: Report

```
Scope Check: CLEAN
```

or:

```
Scope Check: DRIFT DETECTED

Unplanned changes:
- src/utils/format.ts -- refactored date formatting (not in plan)
- tests/helpers.ts -- added test utility (not in plan)

Assessment: The date formatting refactor was a drive-by improvement.
The test utility supports the planned changes. Recommend reverting
the date formatting change to a separate PR.
```

or:

```
Scope Check: REQUIREMENTS MISSING

Missing from plan:
- Step 4: "Add error handling for network timeouts" -- not implemented
- Step 7: "Update API documentation" -- not done

Assessment: Error handling is critical. Documentation can be deferred.
```

---

## When to Run

- After completing any multi-step plan
- Before submitting a PR
- When a task feels like it took longer than expected (often a sign of scope creep)
- During code review of your own work

---

## Common Drift Patterns

### The Drive-By Refactor

While implementing Feature A, the agent notices code in File B that "could be better." It refactors File B. This refactor is untested (because it wasn't in the plan), unreviewed (because it wasn't expected), and may break things (because it wasn't analyzed).

**Rule**: If it's not in the plan, it goes in a separate PR.

### The Premature Optimization

The agent implements the feature, then spends extra time optimizing it. The optimization wasn't requested, isn't measured, and may be premature.

**Rule**: Optimization is a separate task with its own benchmarks.

### The Missing Edge Case

The plan says "handle errors." The agent handles one error type and moves on. The plan's intent was comprehensive error handling.

**Rule**: Vague plan steps produce vague implementations. Better plans produce better results.

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Scope Drift Detection

After completing a plan/PR, compare plan against actual changes:
1. Did we change anything unplanned? (scope creep)
2. Did we miss anything planned? (missing requirements)

Report: Scope Check: CLEAN / DRIFT DETECTED / REQUIREMENTS MISSING
```
