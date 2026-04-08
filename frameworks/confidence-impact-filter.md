# Confidence x Impact Filter

**A quantitative two-dimensional system for filtering code review findings. Inspired by risk assessment methodology.**

---

## The Problem

AI code reviews produce too many findings. Most are noise: style preferences, hypothetical concerns, pre-existing issues, linter-catchable trivia. The signal-to-noise ratio makes the reviews useless.

The solution is not "be more selective" (vague). The solution is a quantitative filter with explicit thresholds.

---

## The Two Dimensions

Every finding is scored on two axes:

### Confidence (0-100)

How certain are you this is a real issue?

| Range | Meaning | Example |
|-------|---------|---------|
| 90-100 | Certain -- can demonstrate the failure | Missing null check on a value documented as nullable |
| 70-89 | High confidence -- strong evidence | Race condition pattern with no synchronization |
| 50-69 | Moderate -- plausible but unverified | Potential memory leak in long-running process |
| 30-49 | Speculative -- based on pattern matching | "This might cause issues under high load" |
| 0-29 | Guess -- no concrete evidence | "I have a feeling this could be problematic" |

### Impact (0-100)

How bad is it if this issue exists and is not fixed?

| Range | Meaning | Example |
|-------|---------|---------|
| 81-100 | Critical -- data loss, security breach, system down | SQL injection, unencrypted credentials |
| 61-80 | High -- significant functionality broken | Payment processing error, data corruption |
| 41-60 | Medium -- degraded experience, workarounds exist | Slow query on main page, broken mobile layout |
| 21-40 | Low -- cosmetic or minor inconvenience | Inconsistent naming, missing log message |
| 0-20 | Trivial -- no user-visible effect | Code style, import ordering |

---

## Progressive Thresholds

The key insight: **high-impact findings deserve reporting even at lower confidence**. A possible security vulnerability is worth mentioning even if you're only 50% sure. A possible style inconsistency is not worth mentioning unless you're 95% sure.

| Impact Level | Minimum Confidence to Report |
|-------------|------------------------------|
| Critical (81-100) | 50% |
| High (61-80) | 65% |
| Medium (41-60) | 75% |
| Low (21-40) | 85% |
| Trivial (0-20) | 95% |

### Visualization

```
Confidence
100 |  REPORT   REPORT   REPORT   REPORT   REPORT
 95 |  report   REPORT   REPORT   REPORT   REPORT
 85 |  ------   report   REPORT   REPORT   REPORT
 75 |  ------   ------   report   REPORT   REPORT
 65 |  ------   ------   ------   report   REPORT
 50 |  ------   ------   ------   ------   report
  0 |  ------   ------   ------   ------   ------
     Trivial    Low     Medium    High    Critical
                        Impact →
```

Findings below the threshold line are **silently discarded**. No "minor note" or "just FYI." Gone.

---

## False Positive Exclusion List

Before scoring, immediately discard findings that match these patterns:

1. **Pre-existing issues**: Problems that existed before the changes under review
2. **Linter-catchable**: Anything a standard linter/formatter would flag (leave it to automation)
3. **Pedantic nitpicks**: Style preferences with no functional impact
4. **Obviously intentional**: Changes that are clearly deliberate design decisions
5. **Hypothetical-only**: "This could be a problem if [unlikely scenario]" with no supporting evidence

---

## Output Format

When reporting findings, include the scores:

```markdown
### Finding: Unvalidated user input in query builder

- **Confidence**: 85/100 -- input flows directly from request body to SQL template
- **Impact**: 95/100 -- SQL injection, potential data breach
- **Threshold**: PASS (Critical impact requires 50% confidence, we have 85%)
- **Recommendation**: Use parameterized queries or input validation middleware
```

Findings that don't pass the threshold simply don't appear in the review.

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Code Review Filtering

Rate each finding on:
- Confidence (0-100): How certain is this real?
- Impact (0-100): How bad if unfixed?

Progressive thresholds:
- Critical impact (81-100): report at 50%+ confidence
- High (61-80): 65%+
- Medium (41-60): 75%+
- Low (21-40): 85%+
- Trivial (0-20): 95%+

Exclude: pre-existing issues, linter-catchable, pedantic nitpicks, obviously intentional.
```

---

## Why This Works

Most "be selective" instructions fail because they rely on the AI's judgment about what's important -- the same judgment that produced the noisy review in the first place. The Confidence x Impact Filter replaces judgment with arithmetic:

- Score both dimensions independently
- Look up the threshold
- Report or discard -- no discretion involved

This eliminates the "just one more thing" problem where low-value findings accumulate because each one seems worth mentioning individually.
