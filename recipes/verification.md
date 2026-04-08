# Verification Rules

Rules for verifying work is actually done, not just "done."

## Core Verification Rules

### Run Tests Before Declaring Done
```markdown
- For code changes: run the project's test suite before declaring done
```
This is non-negotiable. "I think it works" is not verification.

### Validate Configs
```markdown
- For config changes: validate the config actually loads/works
```
Config files are easy to make syntactically valid but semantically broken. Always run a validation command.

### Definition of Done
```markdown
- Definition of done: tests pass + lint clean + no untracked TODOs
```
A clear definition prevents the "one more thing" trap. When these three conditions are met, the work is done.

### Completion Status Labels
```markdown
- Completion status: DONE / DONE_WITH_CONCERNS / BLOCKED / NEEDS_CONTEXT
```
Not everything ends cleanly. These labels give honest status:
- **DONE**: All acceptance criteria met, tests pass
- **DONE_WITH_CONCERNS**: Works, but there's a risk or trade-off to flag
- **BLOCKED**: Cannot proceed without external input/action
- **NEEDS_CONTEXT**: Missing information required to continue

### Own Your Failures
```markdown
- When tests fail: never claim "it's not caused by our changes" unless verified on main
```
The blame-deflection instinct is strong in AI. If tests fail after your changes, the default assumption should be that your changes caused it until proven otherwise.

## Scope Drift Detection

After completing a plan or PR, run this check:

```markdown
1. Did we do anything unplanned? (scope creep)
2. Did we miss anything planned? (missing requirements)

Report: Scope Check: CLEAN / DRIFT DETECTED / REQUIREMENTS MISSING
```

Compare `plan.md` TODO items against `git diff --stat`. If the diff touches files not in the plan, or the plan has unchecked items, flag it.

## Step-Level Observation (ReAct)

For multi-step tasks, verify after EACH step, not just at the end:

```
Step 1: Execute
Step 2: Read the result
Step 3: Does it match expectations?
  - Yes -> continue
  - No -> STOP, diagnose, decide: continue / adjust / revert
Step 4: Record what happened (one line)
```

This catches problems early when they're cheap to fix, not late when they've compounded.

## Adding to Your CLAUDE.md

```markdown
## Verification

- For code changes: run `npm test` before declaring done
- For config changes: run `config-validator` to verify
- Definition of done: tests pass + lint clean + no untracked TODOs
- Completion status: DONE / DONE_WITH_CONCERNS / BLOCKED / NEEDS_CONTEXT
- When tests fail: never claim "not our fault" unless verified on main
```
