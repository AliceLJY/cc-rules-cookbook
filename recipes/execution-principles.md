# Execution Principles

Rules that govern how CC executes tasks. These are foundational -- everything else builds on top.

## Core Rules

### Auto-confirm for Multi-step Tasks
```markdown
- Multi-step tasks: auto-confirm, stop only on error or need input
```
CC should flow through sequential steps without asking "should I continue?" at every point. It stops when something goes wrong or when it genuinely needs human input.

### Failure Escalation
```markdown
- Failure: retry with specified method, but 3 failures -> STOP and escalate
```
Bad work is worse than no work. If CC fails 3 times at the same step, it should stop and explain what's happening rather than producing garbage output.

### Learn from Mistakes
```markdown
- After every mistake: update CLAUDE.md to prevent recurrence
```
This is how your rule file evolves. Every incident that wastes time should produce a new rule. Over time, your CLAUDE.md becomes a battle-tested constitution.

### Use Available Tools
```markdown
- When a tool/skill exists for a task, use it -- don't bypass
```
If there's a configured hook, skill, or MCP tool for something, CC should use it rather than improvising a manual approach.

### Evaluate Before Executing
```markdown
- For large tasks (>100 LOC or 10+ files): evaluate necessity and ROI before starting
```
Prevent wasted effort. A 30-second sanity check before a multi-hour task can save enormous amounts of time.

## How These Work Together

The execution principles form a feedback loop:

```
Execute task
    |
    v
Success? ----yes----> Continue
    |
    no
    |
    v
Retry (up to 3x)
    |
    v
Still failing? -----> STOP + escalate
    |
    v
Post-mortem: update CLAUDE.md with new rule
```

## Adding to Your CLAUDE.md

Copy the rules above into your `## Execution Principles` section. These are universal and work for any project.
