# Lessons Index

34 battle-tested lessons from 500+ Claude Code sessions. Every lesson here was learned the hard way -- something went wrong, time was wasted, or work was lost.

## How to Read These

Each lesson follows the same format:
- **The Problem**: What went wrong
- **The Rule**: The specific rule that prevents it
- **Why It Matters**: Impact if ignored
- **Implementation**: How to add it to your CLAUDE.md

## Lessons by Category

### Safety (3 lessons)

Rules that prevent data loss and irreversible damage.

| Lesson | Summary |
|--------|---------|
| [no-rm-use-trash](./safety/no-rm-use-trash.md) | Never use `rm` -- move files to Trash instead |
| [secret-guard](./safety/secret-guard.md) | Never guess secrets, env vars, or config values |
| [no-force-push](./safety/no-force-push.md) | Never force-push to shared branches |

### Workflow (4 lessons)

Rules that keep the development process smooth.

| Lesson | Summary |
|--------|---------|
| [grep-all-callers](./workflow/grep-all-callers.md) | After API changes, grep every caller |
| [pr-review-workflow](./workflow/pr-review-workflow.md) | Fix on the same PR branch, never open a new one |
| [commit-before-close](./workflow/commit-before-close.md) | Always commit and push before ending a session |
| [check-existing-prs](./workflow/check-existing-prs.md) | Check for existing PRs before creating one |

### Quality (3 lessons)

Rules that improve code and documentation quality.

| Lesson | Summary |
|--------|---------|
| [readme-bilingual](./quality/readme-bilingual.md) | Separate language files for docs |
| [code-evolvability](./quality/code-evolvability.md) | Design code that the next developer can extend |
| [test-before-commit](./quality/test-before-commit.md) | Run tests before every commit |

### Communication (3 lessons)

Rules that improve how CC interacts with humans.

| Lesson | Summary |
|--------|---------|
| [question-before-execute](./communication/question-before-execute.md) | Evaluate necessity before burning tokens on big tasks |
| [neutral-prompts](./communication/neutral-prompts.md) | Use neutral language to get honest analysis |
| [no-sycophancy](./communication/no-sycophancy.md) | Resist the urge to sugarcoat findings |

---

## Writing New Lessons

Use this template:

```markdown
# [Lesson Title]

## The Problem
What went wrong / what was happening

## The Rule
The specific rule that prevents this

## Why It Matters
Impact if ignored

## Implementation
How to add this to your CLAUDE.md or hooks
```

Key principles:
- Every lesson must come from a real incident
- The rule must be specific enough that CC can follow it
- Include the "why" -- CC follows rules better when it understands the reason
