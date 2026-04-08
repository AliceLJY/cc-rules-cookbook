# Completion Status Taxonomy

**Four-level completion reporting that replaces ambiguous "done" with precise status communication.**

---

## The Problem

"Done" is the most overloaded word in software development. When an AI agent says "done," it could mean:

- Everything works perfectly
- The happy path works, edge cases are untested
- The code compiles but has known issues
- The task is blocked and the agent gave up quietly

A single word cannot convey these distinctions. So we use four.

---

## The Four Statuses

### DONE

Everything is complete and verified.

- Tests pass
- Lint is clean
- No untracked TODOs
- The implementation matches the spec/plan
- Edge cases are handled or explicitly out of scope

**When to use**: Only when you would stake your reputation on the result.

### DONE_WITH_CONCERNS

The task is complete but there are risks, caveats, or known limitations.

- Core functionality works
- But: performance may degrade under load
- But: one edge case is handled with a workaround
- But: a dependency is pinned to a specific version for compatibility
- But: the approach works now but won't scale past N users

**When to use**: When shipping is reasonable but the stakeholder should know about trade-offs. Always list the specific concerns.

**Format**:
```
Status: DONE_WITH_CONCERNS

Concerns:
- The retry logic uses a fixed 3-second delay; exponential backoff would be more robust
- Error messages are generic; specific error codes would improve debugging
```

### BLOCKED

Cannot proceed. Something external is needed.

- Missing access credentials
- Waiting for a dependency to be updated
- Architecture question that requires human decision
- Test environment is down
- Requirements are ambiguous and assumptions would be risky

**When to use**: When continuing would require guessing, and guessing could cause damage. Always specify what's needed to unblock.

**Format**:
```
Status: BLOCKED

Blocked on:
- Need database migration approval before modifying the schema
- The staging API returns 403; need updated credentials

Next action: [who needs to do what]
```

### NEEDS_CONTEXT

Insufficient information to even begin meaningfully.

- The request references systems/code not visible in the current context
- Multiple valid interpretations exist and choosing wrong is expensive
- Domain knowledge is required that the agent doesn't have

**When to use**: When starting work would likely produce the wrong thing. Different from BLOCKED in that the issue is informational, not access-related.

**Format**:
```
Status: NEEDS_CONTEXT

Questions:
- Which payment processor are we integrating? (Stripe, PayPal, or custom?)
- Should this support multi-currency, or USD only?
- Is there an existing error handling pattern in this codebase I should follow?
```

---

## Why Four Instead of Two

Most systems use "done" and "not done." This loses critical information:

| Actual State | Binary Report | Impact |
|-------------|--------------|--------|
| Complete with a performance risk | "Done" | Risk ships silently |
| Blocked on credentials | "Not done" | No one knows what to do next |
| Needs clarification | "Not done" | Agent stalls instead of asking |
| Truly complete | "Done" | Indistinguishable from the first row |

Four statuses force the agent to categorize its actual state, making hidden problems visible.

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Verification

- Completion status: DONE / DONE_WITH_CONCERNS / BLOCKED / NEEDS_CONTEXT
- DONE requires: tests pass + lint clean + no untracked TODOs
- DONE_WITH_CONCERNS: list the specific concerns
- BLOCKED: state what's needed to unblock
- NEEDS_CONTEXT: list the specific questions
```
