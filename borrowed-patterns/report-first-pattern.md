# Pattern: Report-First

**Borrowed from**: [OpenClaw-Medical-Skills](https://github.com/FreedomIntelligence/OpenClaw-Medical-Skills) (CUHK-Shenzhen, 869 medical AI skills)
**Adapted for**: Controlling agent output quality in multi-step tasks
**Where it lives**: Any skill or workflow that produces a deliverable

## The Original Idea

Medical AI skills that run lab analyses, literature reviews, or genomic pipelines follow a strict output discipline: **never stream intermediate tool results to the user.** Instead:

1. Create a report template at the start
2. Progressively fill sections as work completes
3. Deliver only the finished report

The user never sees raw tool output, partial results, or debugging noise. They see a structured document that was built incrementally behind the scenes.

## How It's Adapted for Claude Code

When a task involves multiple steps with intermediate outputs (research, analysis, code review, migration), the agent should:

```
❌  Step 1 result → dump to user → Step 2 result → dump to user → "here's everything"
✅  Create structure → fill silently → deliver final output
```

### Practical Rules

1. **Template First**: Before executing, define the output structure (sections, headings, expected content types). Write it to a file if the task is long.

2. **Silent Accumulation**: Tool outputs, intermediate calculations, and partial results go into working memory or a scratch file -- not into the conversation.

3. **Progressive Assembly**: Each step fills its section of the template. The agent knows what's done and what's remaining.

4. **Single Delivery**: The user gets the completed report. If they want intermediate details, they can ask -- but the default is clean output.

### Where It Helps Most

- **Code review skills**: Collect all findings first, then deliver a structured review (not a stream of "I found this... and this... and also this...")
- **Research tasks**: Build `research.md` progressively, deliver when complete
- **Migration tasks**: Accumulate all changes needed, present the migration plan as a whole
- **Multi-file analysis**: Analyze all files, then summarize patterns -- don't narrate each file

## Anti-Pattern

The "thinking out loud" agent that dumps every intermediate result into the conversation, filling context with noise and making the final answer harder to find. Report-First prevents this by separating the working process from the deliverable.

## When to Use

- Any multi-step task where the user cares about the conclusion, not the journey
- Skills that process multiple inputs and produce a single synthesis
- Tasks where intermediate output would confuse more than inform
