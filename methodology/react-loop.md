# ReAct Loop

**Step-level observation and reflection during implementation. From AI research (Yao et al., 2022) adapted to human-AI collaboration.**

---

## The Problem

AI agents execute steps sequentially without checking whether each step actually worked. They plow ahead, accumulating errors, and deliver a final result built on a foundation of unchecked assumptions.

The ReAct loop forces verification at every step.

---

## The Cycle

```
+-------+     +---------+     +---------+     +--------+
|  Act  | --> | Observe | --> | Reflect | --> | Record |
+-------+     +---------+     +---------+     +--------+
    ^                                              |
    |                                              |
    +---------- next step <------------------------+
```

### Act

Execute the current step from the plan.

### Observe

Read the actual result. Not "assume it worked" -- **read it**.

- After a command: check the output and exit code
- After a file edit: verify the edit applied correctly
- After a test run: read every failure, not just the count
- After an API call: check the response body, not just the status code

### Reflect

Compare the observation against expectations:

- **Matches expectations**: Proceed to Record, then next step
- **Minor deviation**: Note the deviation, assess whether it affects downstream steps
- **Major deviation**: **STOP**. Do not proceed. Diagnose the issue first.
- **Complete failure**: **REVERT**. Don't patch a broken step.

Decision tree:

```
Observation matches expected?
├── Yes → Record + Continue
├── Partially → Record deviation + Assess downstream impact
│   ├── No downstream impact → Continue with note
│   └── Downstream impact → Adjust plan before continuing
└── No → STOP
    ├── Root cause clear → Fix + Retry this step
    ├── Root cause unclear → Investigate before proceeding
    └── Wrong approach → REVERT to last known-good state
```

### Record

Mark the step complete and write a one-line observation note:

```markdown
- [x] Step 3: Add validation middleware -- files: `src/middleware/validate.ts`
  [Observation: Added. Tests pass. Validation rejects malformed input as expected.]
```

or, when something unexpected happens:

```markdown
- [x] Step 3: Add validation middleware -- files: `src/middleware/validate.ts`
  [Observation: Validation works but discovered that the existing error handler
   swallows validation errors silently. Added Step 3b to fix error propagation.]
```

---

## Enforcement via Hooks

Written rules get forgotten. Hooks enforce mechanically.

A nag-reminder hook can monitor tool call patterns and detect when the agent has executed 5+ tool calls without recording observations. When detected, it injects a reminder:

```
"You've made several tool calls without recording observations.
Pause and reflect: did each step produce the expected result?"
```

This converts the ReAct loop from a guideline into a mechanical constraint.

---

## The Critical Rule

**Wrong direction -> revert, don't patch.**

When Step 5 reveals that Step 3 was built on a wrong assumption:

- **Wrong**: Fix Step 3's output in Step 5, then continue
- **Right**: Revert to the state before Step 3, fix the assumption, re-execute Steps 3-5

Patches on top of mistakes compound. Each patch introduces its own edge cases. By Step 10, you have a Rube Goldberg machine that nobody can maintain.

---

## Practical Tips

1. **Observations don't need to be long.** One line is enough. "Tests pass." "File created at expected path." "Response returned 200 with expected schema."

2. **Not every step needs deep reflection.** Routine steps (create file, run formatter) get a quick "done as expected." Complex steps (refactor, API integration) get deeper observation.

3. **The value is in the unexpected observations.** "Tests pass" is confirmation. "Tests pass but one test was already failing before my changes" is an insight that prevents a false blame later.

4. **Observations accumulate into documentation.** At the end of implementation, the plan.md with all its observation notes is a record of what actually happened -- decisions, surprises, and deviations from the original plan.

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Implementation Discipline

- Execute plan.md step by step with ReAct loop: Act -> Observe -> Reflect -> Record
- After each step: verify the result matches expectations
- Unexpected result -> STOP and diagnose, don't plow ahead
- Wrong direction -> revert, don't patch
- Record one-line [Observation: ...] notes on each completed step
```
