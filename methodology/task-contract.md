# Task Contracts

A Task Contract defines what "done" means before work begins. It prevents the two most common failure modes: CC declaring "done" prematurely, and scope creeping beyond the original goal.

## The Problem

Without a contract:
- CC writes a stub function and says "done!"
- Or CC keeps polishing endlessly because "done" is undefined
- Or the human and CC have different ideas of what "complete" means

## The Contract

For any complex task, create `{TASK}_CONTRACT.md` before starting:

```markdown
# Contract: [Task Name]

## Goal
[One sentence: what does success look like?]

## Acceptance Criteria
- [ ] [Specific, testable criterion 1]
- [ ] [Specific, testable criterion 2]
- [ ] [Specific, testable criterion 3]

## Verification Commands
- Tests: `npm test`
- Lint: `npm run lint`
- Type check: `npx tsc --noEmit`

## Out of Scope
- [Thing that might seem related but isn't part of this task]
- [Another thing to explicitly exclude]

## Not Done Until
- All acceptance criteria checked
- Verification commands pass
- Scope check: no drift, no missing requirements
```

## Rules

### Agent Must Meet Contract
If acceptance criteria aren't met, the task isn't done. Period. This prevents stub-and-stop behavior.

### Different Contracts, Different Sessions
Each contract should be handled in its own CC session. Mixing unrelated contracts in one session causes context pollution and scope creep.

### Wrong Direction = Revert
If mid-implementation you realize the approach is wrong, revert and restart with a corrected plan. Don't patch on top of a broken foundation.

## Scope Drift Detection

After completing work, run this check against the contract:

```
1. Did we do anything not in the contract? (scope creep)
2. Did we miss anything in the contract? (missing requirements)

Scope Check: CLEAN / DRIFT DETECTED / REQUIREMENTS MISSING
```

## When to Use Contracts

| Situation | Contract? |
|-----------|-----------|
| Quick bug fix (< 20 LOC) | No |
| Feature addition (20-100 LOC) | Recommended |
| Refactoring (100+ LOC) | Yes |
| Multi-file changes | Yes |
| Production-critical changes | Mandatory |
| Anything where "done" is ambiguous | Yes |

## Adding to Your Workflow

Reference in your `workflow.md` or `CLAUDE.md`:

```markdown
## Task Management
- Complex tasks: write {TASK}_CONTRACT.md with acceptance criteria before starting
- Agent not meeting contract = not done
- Different contracts -> different sessions
- Wrong direction -> revert and restart, don't patch
```
