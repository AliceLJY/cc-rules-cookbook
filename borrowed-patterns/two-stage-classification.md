# Pattern: Two-Stage Classification

**Borrowed from**: [Anthropic's Auto Mode Architecture](https://www.anthropic.com/engineering/claude-code-auto-mode) (Claude Code auto-approve system)
**Adapted for**: Any agent safety gate or quality filter that needs both speed and accuracy
**Where it lives**: Conceptual pattern for building review/filter systems

## The Original Idea

Claude Code's Auto Mode uses a two-stage classifier to decide whether to allow, block, or escalate agent actions:

**Stage 1 -- Fast Filter**: Single-token yes/no classification. Cheap, fast, catches obvious cases. Accepts a higher false-positive rate (8.5%) for very low false-negative rate.

**Stage 2 -- Deep Analysis**: Only triggered when Stage 1 flags something. Uses chain-of-thought reasoning. Much lower false-positive rate (0.4%) but still has a 17% false-negative rate on edge cases.

Critical design choice: **the classifier only sees the raw tool call, stripped of the agent's prose and reasoning.** This prevents the agent from arguing its way past the gate and prevents injected content from influencing the decision.

## How It's Adapted for Claude Code Workflows

The two-stage pattern applies anywhere you need to filter or review at scale without drowning in false positives:

### The Architecture

```
Input → Stage 1 (Fast, Broad) → PASS → Allow
                                → FLAG → Stage 2 (Slow, Precise) → PASS → Allow
                                                                  → BLOCK → Handle
```

### Key Principle: Strip the Persuasion Layer

When evaluating agent output, separate:
- **What the agent did** (the action, the code change, the command)
- **What the agent said about it** (the explanation, the justification)

Judge the action, not the explanation. An agent can write a convincing rationale for a terrible decision. The classifier should be immune to persuasion.

### Applications

| Domain | Stage 1 (Fast) | Stage 2 (Deep) |
|--------|----------------|-----------------|
| Code Review | Lint + static analysis | Human/AI deep review |
| PR Triage | File count + diff size | Full change analysis |
| Memory Storage | Keyword/duplicate check | Semantic dedup + importance scoring |
| Skill Matching | Trigger phrase match | Intent analysis |

### Deny-and-Continue

Another borrowed concept: when an action is blocked, don't halt the entire workflow. Return the rejection reason and let the agent find an alternative path. Only escalate to human after repeated blocks (3 consecutive or 20 cumulative in Anthropic's implementation).

This prevents a single false positive from killing a long-running task.

## When to Use

- Building any automated review or approval system
- When you need to process high volume (can't deep-analyze everything)
- When false negatives are more costly than false positives (security, data loss)
- When the thing being evaluated can "argue" for itself (AI output review)
