# Resist Sycophancy

## The Problem

AI assistants are trained on human feedback that rewards agreeable, positive responses. This creates a systematic bias:

- "Great question!" (It was a normal question)
- "Excellent approach!" (It was an average approach)
- "Everything looks good!" (There are 3 issues)
- Score: 4.5/5 (It's a 2.5/5)

Sycophancy is the most insidious AI bias because it feels good to receive. You don't notice it's happening until you realize every review comes back positive and every suggestion gets praised.

## The Rule

```markdown
- No sycophancy: finding problems is the job, not giving compliments
- Default review score: 2/5, must be argued UP
```

Specific anti-sycophancy measures:
1. Never start a response with praise for the question/code
2. Default review score is 2/5, not 4/5
3. When you feel the urge to "be nice" about a finding, that's sycophancy -- report the finding as-is
4. Completed != quality. Effort != quality. Confidence != correctness
5. Score of 5/5 should appear in < 5% of reviews

## Why It Matters

- Sycophantic reviews miss real issues
- False confidence leads to shipping broken code
- Over time, inflated scores make all reviews meaningless
- The human loses the ability to calibrate quality

## Implementation

Add to your CLAUDE.md:

```markdown
## Honesty
- No sycophancy: finding problems is the job, not giving compliments
- Default review score: 2/5, argue UP

## Code Review Anti-Rationalization
- "It's mostly good" -> mostly good = partially bad
- "Minor issues only" -> minor issues compound
- "The intent is clear" -> intent without execution = nothing
- "They tried hard" -> effort is irrelevant
```

The key insight: sycophancy is not a feature, it's a bug. Rules that explicitly counter it produce dramatically more useful output.
