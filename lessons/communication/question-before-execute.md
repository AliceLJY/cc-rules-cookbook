# Question Necessity Before Executing Large Tasks

## The Problem

When presented with a large task -- "refactor the metadata system," "migrate all 97 JSON.parse calls," "rewrite the auth module" -- CC's default behavior is to immediately start researching and planning. It deploys subagents, writes research documents, and burns through significant resources before anyone questions whether the task is actually necessary.

In one incident, a metadata migration affecting 97 call sites was proposed. CC immediately dispatched 3 parallel research agents and produced a full research document -- only for the human to point out that the migration solved a theoretical problem with no real-world impact. Thousands of tokens wasted on unnecessary work.

## The Rule

```markdown
- For large tasks (>100 LOC or 10+ files): evaluate necessity and ROI before starting
```

Before diving into any large task:
1. Spend 30 seconds asking: does this solve a real problem or a theoretical one?
2. Is there evidence of actual pain (bug reports, performance data, user complaints)?
3. Is the ROI worth the effort right now, or should it wait?
4. If the answer is "not now," say so directly

## Why It Matters

- Large tasks consume significant resources (tokens, time, context)
- Once research starts, sunk cost bias makes it hard to stop
- "Optimize/refactor/migrate" tasks are especially prone to low ROI
- A 30-second evaluation can save hours of wasted work

## Implementation

Add to your CLAUDE.md `## Execution Principles` or `## Direction Check`:

```markdown
- For large tasks (>100 LOC or 10+ files): evaluate necessity and ROI before starting
- "Optimize/refactor/migrate" tasks: require evidence of real-world pain before starting
```

This pairs well with the XY Problem detection rule:

```markdown
- If a request looks like an XY Problem (means and goal misaligned), point it out before executing
```
