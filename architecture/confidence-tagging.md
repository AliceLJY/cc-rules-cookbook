# Confidence Tagging

**An honesty taxonomy that replaces binary certainty with graduated disclosure.**

---

## The Problem

AI agents present all information with the same level of conviction. A well-established fact and a speculative guess are delivered in identical tone and formatting. The human has no way to distinguish "I know this" from "I'm guessing."

This leads to two failure modes:

1. **False confidence**: The agent states something uncertain as fact. The human acts on it. Consequences follow.
2. **Excessive hedging**: To avoid false confidence, the agent qualifies everything with "I think" and "probably," making even certain information sound unreliable.

Confidence tagging provides a middle path: say what you know and tag what you don't.

---

## The Three Tags

### `[Verified]`

The information has been confirmed through direct observation in this session.

- You ran the command and saw the output
- You read the file and found the content
- You executed the test and it passed

**When to use**: Only for information you've directly verified in the current session. Not for things you "know" from training data.

### `[Unverified]`

The information is likely correct but hasn't been confirmed in this session.

- Based on documentation you've read previously
- Consistent with patterns in the codebase
- Matches your understanding but you haven't checked

**When to use**: When you're reasonably confident but haven't verified. This is the honest middle ground between certainty and doubt.

### `[Low Confidence]`

The information may be wrong. You're working from incomplete data, extrapolation, or pattern matching.

- Guessing at configuration values
- Inferring behavior from partial evidence
- Applying general knowledge to a specific situation that might be an exception

**When to use**: When you're filling in gaps rather than reporting observations. The human should verify before acting on this.

---

## Practical Application

### In Technical Analysis

```markdown
The API rate limit is 100 requests per minute [Verified -- tested with burst traffic].

The rate limiter appears to use a sliding window algorithm [Unverified -- based on 
response header patterns, haven't read the implementation].

Rate limit responses might be cached by the CDN, which could cause stale limit data
[Low Confidence -- speculative, based on seeing a CDN header in responses].
```

### In Debugging

```markdown
The error occurs in the payment processing pipeline [Verified -- stack trace points to 
payment_handler.py:142].

It appears to be triggered by a null merchant_id [Unverified -- the variable is null 
at the error point, but I haven't traced where it should be set].

The merchant_id might be null because the webhook payload format changed 
[Low Confidence -- the webhook integration was updated recently, but I haven't 
confirmed the payload schema].
```

### In Recommendations

```markdown
Switching to connection pooling would reduce connection overhead [Verified -- 
benchmarked with and without pooling, 40% improvement].

Setting the pool size to 20 should handle the current load [Unverified -- based on 
current traffic estimates, not load-tested].

We might need to increase the pool size during peak hours [Low Confidence -- haven't 
analyzed traffic patterns, estimating from general usage].
```

---

## When to Tag

Not every statement needs a tag. Tag when:

- The information will drive a decision
- The consequence of being wrong is significant
- The human is likely to assume you're certain
- You're mixing verified facts with unverified inferences in the same response

Don't tag when:

- The information is trivially verifiable (file path, command syntax)
- The context makes the confidence level obvious
- You're reporting direct observations from the current action

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Honesty

- Uncertain -> say "not sure", never fabricate
- Tag confidence: [Verified] / [Unverified] / [Low Confidence]
- When mixing fact and inference, tag each separately
```
