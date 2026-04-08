# Code Review Standards

Rules for making AI-assisted code review actually useful rather than sycophantic noise.

## The Problem with AI Code Review

By default, AI reviewers:
- Give inflated scores (4/5 or 5/5 on mediocre code)
- Praise effort instead of evaluating output
- Report style nitpicks as "findings" while missing real bugs
- Accept confident-looking code as correct

These rules fix that.

## Bias Correction Matrix

### 7 AI Review Biases

| Bias | How It Corrupts Reviews | Countermeasure |
|------|------------------------|----------------|
| **Sycophancy** | Sugarcoats problems, weakens severity | Finding problems is the job. No compliments |
| **Length Bias** | "Long = thorough" assumption | Penalize verbosity. Concise > lengthy |
| **Authority Bias** | Confident tone = correct conclusion | Verify every claim. Confidence means nothing |
| **Completion Bias** | "They finished it" = good work | Completion != quality |
| **Effort Bias** | "They tried hard" = be lenient | Effort is irrelevant. Judge OUTPUT only |
| **Recency Bias** | New pattern = better pattern | Existing patterns exist for a reason |
| **Familiarity Bias** | "I've seen this before" = it's fine | Common != correct |

### Anti-Rationalization Rules

When you catch yourself thinking:
1. "It's mostly good" -> Mostly good = partially bad = **needs work**
2. "Minor issues only" -> Minor issues compound into major failures
3. "The intent is clear" -> Intent without execution = nothing
4. "Could be worse" -> "Could be worse" is not a quality bar
5. "They tried hard" -> Effort is irrelevant
6. "It's a first draft" -> Evaluate what exists, not potential

### Score Calibration

- **Default score: 2/5** -- must be argued UP, not down from 5
- 5/5 should appear in **< 5%** of reviews
- When you feel the urge to "be nice" -> lower the score (that's sycophancy)

## Confidence x Impact Filtering

Every finding gets two scores:

### Confidence (0-100)
How certain is this actually a problem?
- 100 = Reproducible, provable
- 70 = Likely a problem, needs context to confirm
- 40 = Might be a problem, might be intentional
- 10 = Gut feeling, no evidence

### Impact (0-100)
How bad if left unfixed?
- 100 = Data loss, security vulnerability, service outage
- 70 = Feature broken, severe performance degradation
- 40 = Maintainability issue, code standard violation
- 10 = Style preference, no functional effect

### Progressive Thresholds

Higher impact = lower confidence required (because high-impact issues are worth flagging even with uncertainty):

| Impact Level | Range | Min Confidence |
|-------------|-------|----------------|
| Critical | 81-100 | 50 |
| High | 61-80 | 65 |
| Medium | 41-60 | 75 |
| Medium-Low | 21-40 | 85 |
| Low | 0-20 | 95 |

### False Positive Exclusions

Even if scores pass thresholds, do NOT report:
1. Pre-existing issues not introduced by this change
2. Things that look like bugs but are intentional
3. Nitpicks a senior engineer wouldn't flag
4. Issues the linter/compiler already catches
5. Generic code quality opinions without specific evidence
6. Explicitly lint-suppressed code
7. Obviously intentional changes

## Report Format

```markdown
## Review Summary
[One-line conclusion: pass / conditional pass / fail]
Score: X/5 (default 2, argued from there)

## Findings (by impact, descending)
[Impact: XX] [Confidence: XX] Issue description
-> Suggested fix

## Scope Check
CLEAN / DRIFT DETECTED / REQUIREMENTS MISSING
```

## Adding to Your CLAUDE.md

The full framework above may be too verbose for inline CLAUDE.md. Use this compressed version:

```markdown
## Code Review
- Default score: 2/5, argue UP
- Rate findings: [Impact: X] [Confidence: X], filter by threshold
- No sycophancy: finding problems is the job, not giving compliments
- Exclude: pre-existing issues, linter-covered items, style nitpicks
```
