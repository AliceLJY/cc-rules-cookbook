# Use Neutral Prompts for Honest Analysis

## The Problem

AI assistants want to please. When you say "find the bugs in this code," CC will find bugs even if there aren't any -- fabricating plausible-sounding issues to fulfill the request. Similarly, "what's wrong with this architecture?" prompts CC to invent problems.

This isn't dishonesty; it's a systematic bias toward giving the user what they seem to want.

## The Rule

```markdown
- Use neutral prompts: not "find problems," say "read through the logic, report all findings"
- Don't preset conclusions: not "this code has perf issues," say "analyze this code's performance characteristics"
```

### Biased vs. Neutral Prompts

| Biased (avoid) | Neutral (prefer) |
|----------------|------------------|
| "Find the bugs" | "Read through the logic, report all findings" |
| "What's wrong with this?" | "Analyze this code and report what you observe" |
| "This has performance issues" | "Analyze the performance characteristics" |
| "Review and approve this PR" | "Review this PR, score 2/5 by default, argue up" |
| "Is this a good approach?" | "What are the trade-offs of this approach?" |

## Why It Matters

- Biased prompts produce biased findings -- garbage in, garbage out
- Fabricated bugs waste time investigating non-issues
- False positives erode trust in the review process
- Neutral prompts get you the truth, not what you want to hear

## Implementation

This is more about YOUR behavior than CLAUDE.md rules, but you can add:

```markdown
## Code Review
- Default score: 2/5, argue UP
- Use neutral framing: "report findings" not "find problems"
```

For deep verification, use adversarial mode: have one CC session write the code and a different session review it without knowing who wrote it.
