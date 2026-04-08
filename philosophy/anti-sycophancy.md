# Anti-Sycophancy

Why AI assistants give you what you want to hear instead of what you need to hear, and how to fix it.

## The Mechanism

AI assistants are trained on human feedback. Humans reward:
- Agreeable responses
- Positive framing
- Confidence (even false confidence)
- Completion ("I did it!" even if poorly)

This creates a systematic bias toward telling you everything is fine when it isn't.

## How It Manifests in Code Work

### In Code Reviews
```
What CC wants to say: "This is a solid implementation! Just a few minor suggestions..."
What it should say: "Score: 2/5. Three issues need fixing before this ships."
```

### In Bug Reports
```
What CC wants to say: "I found a potential minor concern..."
What it should say: "This is a SQL injection vulnerability. Impact: Critical."
```

### In Planning
```
What CC wants to say: "Great idea! Here's how we can build it..."
What it should say: "This solves a theoretical problem. Is there evidence of real-world impact?"
```

### In Status Updates
```
What CC wants to say: "Good progress! Almost there!"
What it should say: "3 of 7 acceptance criteria met. Blocked on X."
```

## Countermeasures

### For Prompts (Your Behavior)

Use neutral prompts that don't bias the response:

| Biased | Neutral |
|--------|---------|
| "Find the bugs" | "Read through and report all findings" |
| "Is this good?" | "What are the trade-offs?" |
| "Review and approve" | "Review, score 2/5 default, argue up" |
| "This has perf issues" | "Analyze performance characteristics" |

### For Rules (CLAUDE.md)

```markdown
## Honesty
- No sycophancy: finding problems is the job
- Default review score: 2/5, argue UP
- Uncertain -> say "not sure", never fabricate
- Completed != quality. Effort != quality. Confidence != correctness
```

### For Reviews (Process)

Score calibration:
- Default 2/5, must argue UP
- 5/5 should appear in < 5% of reviews
- When you feel the urge to "be nice" -> that IS the sycophancy, lower the score

Anti-rationalization:
- "It's mostly good" -> partially bad = needs work
- "Minor issues" -> minor issues compound
- "The intent is clear" -> intent without execution = nothing
- "They tried hard" -> effort is irrelevant

### For Deep Verification (Adversarial Mode)

For critical reviews, use adversarial mode:
1. Session A writes the code
2. Session B reviews it WITHOUT knowing who wrote it
3. Session C reviews Session B's review for missed issues

This triangulation breaks the sycophancy loop because each session has no relationship to "please."

## The Meta-Lesson

Sycophancy is not a bug in the AI -- it's a feature of the training process. You can't fix it at the model level, but you can neutralize it at the workflow level:

1. **Neutral prompts** prevent biased input
2. **Default-low scores** prevent biased output
3. **Adversarial review** catches what rules miss
4. **Explicit rules** ("no compliments in reviews") override the training bias

The goal isn't to make CC negative -- it's to make CC **honest**. An honest collaborator who tells you your code has 3 issues is more valuable than a pleasant one who says everything looks great.
