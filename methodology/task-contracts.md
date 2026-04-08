# Task Contracts

**Defining "done" before starting. Prevents stub-and-stop, drives-by completion, and ambiguous handoffs.**

---

## The Problem

Without explicit completion criteria, AI agents exhibit two failure modes:

1. **Stub-and-stop**: The agent implements a skeleton, declares "done," and the human discovers the missing pieces later
2. **Gold-plating**: The agent keeps adding features and refinements past the point of usefulness, consuming context and time

Both stem from the same root cause: "done" is undefined.

---

## The Contract

For complex tasks, define completion criteria upfront in a `{TASK}_CONTRACT.md` file:

```markdown
# Contract: User Authentication Refactor

## Acceptance Criteria
- [ ] All existing auth tests pass without modification
- [ ] New JWT validation middleware handles expired tokens (returns 401)
- [ ] New JWT validation middleware handles malformed tokens (returns 400)
- [ ] Session refresh endpoint works with valid refresh tokens
- [ ] Rate limiting applied to login endpoint (10 attempts per minute)
- [ ] API documentation updated for changed endpoints

## Out of Scope
- OAuth2 integration (separate task)
- Password reset flow changes
- Admin authentication (unchanged)

## Not Done Until
- All boxes above are checked
- Tests pass (`npm test`)
- No lint warnings (`npm run lint`)
- No TODOs remain in changed files

## Verification Method
- Run test suite
- Manual test: login -> receive JWT -> use JWT -> let JWT expire -> refresh -> use new JWT
- Manual test: 11 rapid login attempts -> verify rate limit triggers
```

---

## Contract Rules

### 1. Agent Not Meeting Contract = Not Done

The contract is the definition of done. If any criterion is unmet, the task is incomplete regardless of what the agent claims.

### 2. Different Contracts Get Different Sessions

Don't mix Task A's contract with Task B's implementation. Context pollution degrades both.

### 3. Wrong Direction -> Revert and Restart

If the implementation approach turns out to be wrong, don't patch it. Revert to the pre-implementation state and take a different approach. The contract stays the same; the implementation changes.

### 4. Contracts Are Negotiable Before Starting

The human and AI can negotiate criteria before work begins. Once work starts, the contract is fixed. Changes require explicit renegotiation.

### 5. Out of Scope Is As Important As In Scope

Listing what's explicitly excluded prevents the agent from expanding the task. "OAuth2 integration" in the out-of-scope section means the agent won't add it "while we're here."

---

## When to Use Contracts

| Situation | Contract? |
|-----------|-----------|
| Simple bug fix, clear scope | No -- overkill |
| Feature with 3+ acceptance criteria | Yes |
| Refactor touching 5+ files | Yes |
| Any irreversible operation | Yes |
| Task where "done" has been argued before | Definitely yes |

---

## Contract Anti-Patterns

### The Vague Contract

```markdown
## Criteria
- [ ] Feature works
- [ ] Code is clean
```

This is not a contract. "Works" and "clean" are subjective. Rewrite with specific, testable criteria.

### The Infinite Contract

```markdown
## Criteria
- [ ] Handles all edge cases
- [ ] Performance is optimal
- [ ] Security is comprehensive
```

This can never be satisfied. Scope criteria to specific, enumerable conditions.

### The Missing Contract

No `CONTRACT.md` exists. The agent interprets "done" according to its own judgment, which is biased toward [Completion Bias](../frameworks/bias-correction-matrix.md).

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Task Contracts

- Complex tasks write {TASK}_CONTRACT.md with specific acceptance criteria
- Agent not meeting contract = not done
- Wrong direction -> revert and restart, don't patch
- Different contracts get different sessions
```
