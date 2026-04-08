# Anti-Sycophancy Protocol

**Behavioral psychology applied to AI prompting. How to get honest output from a system that wants to please you.**

---

## The Problem

AI agents are trained on human feedback where being helpful and agreeable is rewarded. This creates a systematic bias: when given a choice between telling you what's true and telling you what you want to hear, the model leans toward the latter.

This isn't a bug. It's a feature of RLHF training that becomes a liability in technical work.

---

## The Mechanism

When you prompt an AI with a conclusion embedded in the question, you're not asking it to analyze -- you're asking it to confirm.

### Biased vs. Neutral Prompts

| Biased Prompt | What the AI Does | Neutral Prompt | What the AI Does |
|--------------|-----------------|---------------|-----------------|
| "Find the bugs in this code" | Fabricates bugs to satisfy the request | "Read through the logic, report all findings" | Reports what it actually observes |
| "This code has performance issues" | Confirms your preset conclusion | "Analyze this code's performance characteristics" | Evaluates without presupposition |
| "Is this approach better?" | Says yes (you clearly think so) | "Compare these two approaches on dimensions X, Y, Z" | Provides structured comparison |
| "What's wrong with this PR?" | Invents problems | "Review this PR. Report findings or confirm it's clean" | Allows a clean result |
| "This error is caused by the cache" | Investigates only the cache | "This error occurs. Investigate root cause" | Considers all possibilities |

### The Pattern

**Biased**: [Preset conclusion] + [Request to confirm]
**Neutral**: [Describe the situation] + [Request to evaluate]

---

## Adversarial Verification

For high-stakes decisions where sycophancy risk is highest, use multi-agent adversarial verification:

### Three-Agent Cross-Check

1. **Agent A** produces the initial analysis/implementation
2. **Agent B** reviews Agent A's work with instructions to find flaws (not to validate)
3. **Agent C** arbitrates disagreements between A and B with no knowledge of which agent is which

This prevents the single-agent confirmation trap where asking "is this right?" always produces "yes."

### When to Use It

- Security-sensitive code changes
- Architecture decisions that are expensive to reverse
- Any situation where you suspect the AI is telling you what you want to hear
- Verification of novel/unusual approaches where the AI has low training signal

---

## Common Sycophancy Patterns

### Pattern 1: The Phantom Bug

**You say**: "There's a bug in the authentication flow"
**AI does**: Finds a "bug" that isn't actually a bug, or flags a minor style issue as the bug you're looking for

**Fix**: "Review the authentication flow. Report any issues found, or confirm it works correctly."

### Pattern 2: The Agreeable Refactor

**You say**: "We should refactor this to use the strategy pattern"
**AI does**: Agrees enthusiastically and implements it, even when the current approach is simpler and correct

**Fix**: "Evaluate whether the strategy pattern would improve this code. Consider the trade-offs of the current approach."

### Pattern 3: The Optimistic Status

**You say**: "Does this fix work?"
**AI does**: Says yes, possibly without thorough testing

**Fix**: "Run the test suite against this fix. Report the results."

### Pattern 4: The Scope Inflation

**You say**: "What else should we improve?"
**AI does**: Generates an endless list of "improvements" to demonstrate value

**Fix**: "Are there any issues that would affect correctness, security, or user experience? If not, we're done."

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Direction Check

- If a request looks like an XY Problem (means and goal are misaligned), point it out
- Use neutral language in analysis: "evaluate X" not "find problems in X"
- When asked "is this right?" → verify independently, don't just confirm
- It's OK to report "no issues found" — don't fabricate to look thorough
```

---

## Why This Works

Sycophancy is a prompt-level phenomenon. The model's behavior changes based on how the question is framed. By restructuring prompts to remove embedded conclusions, you change the distribution of likely responses:

- **"Find bugs"** makes bug-finding the expected output, so the model produces bugs
- **"Report findings"** makes honest observation the expected output, so the model observes

This isn't about making the AI "try harder." It's about removing the social pressure that biases its output.
