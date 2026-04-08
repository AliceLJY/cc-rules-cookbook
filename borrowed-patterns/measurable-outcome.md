# Pattern: Measurable Outcome

**Borrowed from**: [OpenClaw-Medical-Skills](https://github.com/FreedomIntelligence/OpenClaw-Medical-Skills) (CUHK-Shenzhen, 869 medical AI skills)
**Adapted for**: Defining verifiable completion criteria for skills and tasks
**Where it lives**: Skill frontmatter, task contracts, completion verification

## The Original Idea

Every medical AI skill in the OpenClaw collection includes a `measurable_outcome` field in its frontmatter. Not a vague description of what the skill does, but a **concrete, testable success criterion**:

```yaml
measurable_outcome: "Produce a filtered .h5ad file with < 5% doublet rate within 20 minutes"
```

```yaml
measurable_outcome: "Generate a FHIR-compliant patient summary with all required fields populated"
```

The skill knows what success looks like before it starts. After execution, it can self-check: did I produce the thing? Does it meet the criteria?

## How It's Adapted for Claude Code

### For Skills

Every skill should define what "done" looks like in measurable terms. Not "helps with code review" but:

```
measurable_outcome: "Produce a structured review with ≥3 findings rated by confidence×impact, zero unsubstantiated claims"
```

This does three things:
1. **Guides the agent**: it knows what to produce
2. **Enables self-check**: the agent can verify its own output against the criteria
3. **Enables evaluation**: humans can quickly judge pass/fail instead of reading everything

### For Task Contracts

The `{TASK}_CONTRACT.md` pattern already defines completion criteria. Measurable Outcome strengthens it by requiring **quantifiable or binary** criteria:

| Vague Criterion | Measurable Outcome |
|----------------|-------------------|
| "Code should be clean" | "Zero lint errors, all functions < 50 lines" |
| "Tests should pass" | "All existing tests pass + ≥2 new tests covering the changed behavior" |
| "Performance should be good" | "Response time < 200ms at p95 for the target endpoint" |
| "Documentation updated" | "README reflects new API, all new public functions have JSDoc" |

### For Agent Routing

When an agent needs to pick which skill to use, measurable outcomes act as a matching signal. The agent doesn't just match on description keywords -- it matches on whether the skill's defined outcome aligns with the task's goal.

### The Self-Check Loop

```
Define outcome → Execute task → Compare result against outcome → 
  Match? → DONE
  No match? → Diagnose gap → Retry or escalate
```

This is more reliable than asking the agent "are you done?" because it has a concrete reference point instead of vibes.

## Anti-Pattern

The "I did the thing" completion report:
- "Updated the configuration" -- but does it work?
- "Fixed the bug" -- but does the test pass?
- "Wrote the documentation" -- but does it cover the new API?

Measurable Outcome forces the answer to be checkable, not just claimable.

## When to Use

- Writing skill definitions (put it in the frontmatter or description)
- Creating task contracts for complex implementations
- Any time "done" is ambiguous and you want a mechanical check
- Evaluating whether a skill or agent is actually producing value
