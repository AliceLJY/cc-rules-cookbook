# Grep All Callers After API Changes

## The Problem

When modifying a function signature or adding parameters, CC tends to update only the call site that triggered the change. But most functions have multiple callers, and leaving some of them with the old signature creates runtime errors that surface later.

In one incident, a function gained a new `excludeInactive` parameter. It was passed correctly at 2 call sites but missed at 3 others. The PR went through multiple rounds of review before all call sites were caught -- each round finding more missed callers.

## The Rule

```markdown
- Change API/add params -> grep ALL callers, confirm each is updated
```

After modifying any function signature:
1. Run `grep -r "function_name"` across the entire codebase
2. List every caller found
3. For each caller, confirm whether it needs updating
4. If the diff shows 2 callers modified but grep finds 5, explain why the other 3 don't need changes

## Why It Matters

- Partial updates are the #1 cause of "it works for me but breaks in production"
- Runtime errors from missing parameters are harder to debug than compile-time errors
- Each round of review that catches a missed caller wastes reviewer time and delays the PR
- The grep takes 2 seconds; the debugging takes hours

## Implementation

Add to your CLAUDE.md `## ALWAYS` section:

```markdown
- Change API/add params -> grep ALL callers, confirm each is updated
```

You can also add a pre-commit check:

```bash
# In your PR checklist or CI:
# If the diff changes a function signature, require a comment listing all callers
```

### Verification Pattern

After making the change, include this in your PR description:

```markdown
## Caller Audit
Function: `processUser(id, options)`
Added param: `options.includeArchived`

Callers found (grep -r "processUser"):
- src/api/users.ts:42 -- UPDATED
- src/api/admin.ts:88 -- UPDATED
- src/jobs/cleanup.ts:15 -- NOT UPDATED (uses batch path, doesn't apply)
- tests/users.test.ts:23 -- UPDATED
```
