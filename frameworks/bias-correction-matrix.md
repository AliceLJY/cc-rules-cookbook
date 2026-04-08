# Bias Correction Matrix

**Cognitive psychology applied to AI code review. Seven biases that systematically corrupt AI evaluation, with mechanical countermeasures.**

---

## The Problem

AI models trained on human feedback develop systematic biases that make their code reviews unreliable. They praise when they should critique. They report confidence when they should report uncertainty. They mark "complete" when they should mark "broken."

These aren't random errors -- they're predictable patterns. And predictable patterns can be countered.

---

## The Seven Biases

### 1. Sycophancy Bias

**What it does**: The model praises code to maintain a positive interaction.

**Countermeasure**: "Praise is not your job. Your job is finding problems."

Default posture is critical. Compliments must be earned and are never required.

### 2. Length Bias

**What it does**: The model treats longer, more verbose code as more thorough or correct.

**Countermeasure**: "Penalize verbosity. Concise > lengthy."

A 10-line solution that works is superior to a 50-line solution that works. Brevity is a quality signal, not a deficit.

### 3. Authority Bias

**What it does**: The model treats confident-sounding code (strong naming, assertive comments) as more likely correct.

**Countermeasure**: "Confidence means nothing. Confidence is not correctness."

Evaluate the logic, not the tone. A function named `ensureSafeTransaction` can still have a SQL injection vulnerability.

### 4. Completion Bias

**What it does**: The model gives credit for code that is "finished" regardless of quality.

**Countermeasure**: "Completion is not quality. Garbage can be complete."

A fully implemented feature with no error handling, no tests, and hardcoded values is complete. It is also unacceptable.

### 5. Effort Bias

**What it does**: The model softens criticism when it can tell significant effort was invested.

**Countermeasure**: "Effort is irrelevant. Judge OUTPUT only."

A developer spending 40 hours on a solution does not make the solution better than one written in 2 hours. Evaluate the artifact, not the process.

### 6. Recency Bias

**What it does**: The model treats newer patterns, libraries, or approaches as inherently superior.

**Countermeasure**: "New is not better."

The latest framework is not automatically the right choice. Evaluate fitness for purpose, not release date.

### 7. Familiarity Bias

**What it does**: The model treats common patterns as correct because they appear frequently in training data.

**Countermeasure**: "Common is not correct."

A pattern appearing in 10,000 Stack Overflow answers can still be an anti-pattern. Frequency of use is not validation.

---

## The Six Anti-Rationalization Rules

After identifying issues, models rationalize them away. These rules block the six most common rationalizations:

| # | Rationalization | Counter |
|---|----------------|---------|
| 1 | "It's mostly good" | Mostly good = partially bad = **FAIL** |
| 2 | "Minor issues only" | Minor issues compound into major failures |
| 3 | "The intent is clear" | Intent without execution = nothing |
| 4 | "Could be worse" | "Worse" is not the standard |
| 5 | "They tried hard" | Effort is irrelevant. Judge results only |
| 6 | "It's a first draft" | Evaluate what exists now, not its potential |

---

## Scoring Protocol

**Default score: 2/5.** Every review starts here. The reviewer must argue the score UP with evidence.

- 5/5 should appear in fewer than 5% of reviews
- A score of 4/5 or higher requires explicit justification for each point above 2
- "No issues found" is not a valid justification -- it means the review was insufficiently thorough

### Scoring Rubric

| Score | Meaning |
|-------|---------|
| 1/5 | Fundamentally broken -- logic errors, security holes, or architectural violations |
| 2/5 | Works but has real issues that need addressing |
| 3/5 | Solid with minor improvements needed |
| 4/5 | Production-ready, well-structured, handles edge cases |
| 5/5 | Exceptional -- elegant, efficient, comprehensive, and educational |

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Code Review Standards

When reviewing code, guard against these biases:
- Sycophancy: Finding problems is the job, not giving compliments
- Completion bias: Finished != good. Evaluate quality, not effort
- Authority bias: Confident code is not necessarily correct code

Default score: 2/5. Must argue UP with evidence.

Anti-rationalization: "mostly good" = FAIL. "minor issues" = compound risk.
```

---

## Why This Works

Traditional code review instructions say "be thorough." This is vague and unenforceable. The Bias Correction Matrix gives the AI a specific cognitive framework:

1. **Named biases** create categories the model can check against
2. **Mechanical countermeasures** replace judgment calls with rules
3. **Default-low scoring** forces evidence-based evaluation
4. **Anti-rationalization rules** block the escape routes models use to avoid harsh assessments

The result: reviews that find real issues instead of producing polite summaries.
