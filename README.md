# cc-rules-cookbook

**Battle-tested rules from 500+ Claude Code sessions -- your shortcut to CC mastery**

<p align="center"><a href="README_CN.md">🇨🇳 中文版</a></p>

[![Rules](https://img.shields.io/badge/rules-30%2B-blue)](./recipes/)
[![Lessons](https://img.shields.io/badge/lessons-34-green)](./lessons/)
[![Methodology](https://img.shields.io/badge/methodology-3%20stages-orange)](./methodology/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## Why Rules Matter

Claude Code without rules is like a powerful car without a steering wheel -- it has incredible capabilities but no reliable direction. After 500+ sessions, we learned that:

- **CC without rules** produces inconsistent results, makes destructive mistakes, and requires constant babysitting
- **CC with good rules** becomes a disciplined collaborator that follows your workflow, catches its own errors, and improves over time
- **The best rules come from real mistakes** -- every rule in this cookbook was born from an actual incident where things went wrong

The difference between a frustrating CC experience and a productive one is almost always the quality of your `CLAUDE.md`.

---

## Quick Start

Create a `CLAUDE.md` file in your project root with these 10 lines to get immediate improvement:

```markdown
# Project Rules

## Execution
- Multi-step tasks: auto-confirm, stop only on error or need input
- After every mistake: update CLAUDE.md to prevent recurrence

## Honesty
- Uncertain: say "not sure", never fabricate

## Safety
- Never use `rm` to delete files (use `mv` to `~/.Trash/`)
- Never commit untested code
- Change API/add params: grep ALL callers and confirm each

## Verification
- Run tests/lint before declaring done
- After tool call: read the actual result; if unexpected, stop and rethink
```

Then run `claude` in that directory. That's it -- CC reads `CLAUDE.md` automatically.

For the full annotated template, see [`templates/CLAUDE.md.template`](./templates/CLAUDE.md.template).

---

## Cookbook Structure

| Directory | What's Inside | Start Here |
|-----------|--------------|------------|
| [`templates/`](./templates/) | Ready-to-use CLAUDE.md and workflow templates | [CLAUDE.md.template](./templates/CLAUDE.md.template) |
| [`recipes/`](./recipes/) | Individual rule categories (execution, honesty, safety, etc.) | [execution-principles.md](./recipes/execution-principles.md) |
| [`lessons/`](./lessons/) | 34 battle-tested lessons from real incidents | [Lessons Index](./lessons/README.md) |
| [`methodology/`](./methodology/) | The 3-stage workflow system | [research-plan-implement.md](./methodology/research-plan-implement.md) |
| [`philosophy/`](./philosophy/) | Deeper thinking about AI collaboration | [anti-sycophancy.md](./philosophy/anti-sycophancy.md) |

---

## The 3-Stage Methodology

Complex tasks should follow three stages. Skipping stages is the #1 source of wasted work.

```
+-------------------+     +-------------------+     +-------------------+
|                   |     |                   |     |                   |
|    1. RESEARCH    |---->|     2. PLAN       |---->|   3. IMPLEMENT    |
|                   |     |                   |     |                   |
| - Read code/docs  |     | - Write plan.md   |     | - Execute plan    |
| - Produce         |     | - Code snippets   |     | - Step by step    |
|   research.md     |     | - File paths      |     |                   |
| - Human reviews   |     | - TODO list       |     |  +-------------+ |
|                   |     | - Annotation loop |     |  | ReAct Loop  | |
|                   |     |   (1-N rounds)    |     |  |             | |
|                   |     | - NO code changes |     |  | Act         | |
|                   |     |                   |     |  | Observe     | |
|                   |     |                   |     |  | Reflect     | |
|                   |     |                   |     |  | Record      | |
|                   |     |                   |     |  +-------------+ |
+-------------------+     +-------------------+     +-------------------+
```

**Key principle**: Research and implementation happen in sequence, never mixed. Mixing them pollutes context and leads to confused output.

See [`methodology/`](./methodology/) for detailed guides on each stage.

---

## Top 10 Lessons

The most impactful rules distilled from 500+ sessions:

| # | Lesson | Category | Impact |
|---|--------|----------|--------|
| 1 | [Never use `rm` -- use Trash](./lessons/safety/no-rm-use-trash.md) | Safety | Prevented data loss multiple times |
| 2 | [Grep all callers after API changes](./lessons/workflow/grep-all-callers.md) | Workflow | Eliminated incomplete refactors |
| 3 | [Question necessity before executing](./lessons/communication/question-before-execute.md) | Communication | Saved thousands of wasted tokens |
| 4 | [Test before commit](./lessons/quality/test-before-commit.md) | Quality | Stopped broken code from shipping |
| 5 | [PR review: append, don't create new](./lessons/workflow/pr-review-workflow.md) | Workflow | Preserved review history |
| 6 | [Design for evolvability](./lessons/quality/code-evolvability.md) | Quality | Better code architecture |
| 7 | [README: separate languages](./lessons/quality/readme-bilingual.md) | Quality | Professional documentation |
| 8 | [Check existing PRs first](./lessons/workflow/check-existing-prs.md) | Workflow | Avoided duplicate work |
| 9 | [Use neutral prompts](./lessons/communication/neutral-prompts.md) | Communication | More accurate code reviews |
| 10 | [Read tool output completely](./recipes/reply-discipline.md) | Discipline | Caught errors early |

---

## Rule Writing Guide

### How to Write Effective CLAUDE.md Rules

**Do:**
- Write rules from real incidents, not theory
- Keep each rule to 1-2 lines
- Include the "why" -- CC follows rules better when it understands the reason
- Use imperative mood: "Never use rm" not "rm should not be used"
- Group related rules under clear headers
- Update rules when new incidents happen

**Don't:**
- Write essay-length rules -- CC skims long text just like humans do
- Include rules you've never needed -- every rule adds cognitive load
- Copy generic advice -- your rules should reflect YOUR workflow
- Forget to remove outdated rules -- stale rules dilute important ones

### Rule Anatomy

The best rules follow this pattern:

```markdown
## [Category]

- [What to do/not do] ([why/what happens otherwise])
```

Example:
```markdown
## Safety

- Never use `rm` to delete files (use `mv` to `~/.Trash/` -- rm is unrecoverable)
- Change API/add params: grep ALL callers and confirm each (partial updates cause runtime errors)
```

### Anti-Patterns

```markdown
# Too vague -- CC can't act on this
- Write good code

# Too long -- CC will skim past it
- When modifying any function that accepts parameters, you should always check
  every single file in the entire codebase to make sure that no other file
  references this function with the old parameter signature, because if you
  don't, you might end up with runtime errors that are hard to debug...

# Contradictory -- CC gets confused
- Always ask before making changes
- Multi-step tasks: auto-confirm, don't stop
```

---

## CLAUDE.md Template

The full annotated template is at [`templates/CLAUDE.md.template`](./templates/CLAUDE.md.template). It includes:

- Project description placeholder
- Execution principles
- Honesty rules
- NEVER/ALWAYS lists with explanations
- Verification rules
- Reply discipline
- Coding conventions placeholder

Each section has `<!-- CUSTOMIZE: ... -->` comments explaining what to change for your project.

---

## Contributing

Found a rule that saved your bacon? Open a PR! Each lesson should follow the format in [`lessons/README.md`](./lessons/README.md).

---

## Companion Repos

- [**cc-hooks-gallery**](https://github.com/AliceLJY/cc-hooks-gallery) -- Pre-built hooks for Claude Code automation

---

## License

MIT -- take what works, leave what doesn't.

---

> *"The rules you write after a disaster are worth ten times the rules you write before one."*
>
> -- Wisdom from 500+ Claude Code sessions
