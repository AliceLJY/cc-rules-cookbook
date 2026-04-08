# Honesty Rules

Rules that prevent CC from fabricating, guessing, or overselling its confidence.

## Core Rules

### Never Fabricate
```markdown
- Uncertain -> say "not sure", never fabricate
```
This is the single most important honesty rule. AI assistants have a strong tendency to sound confident even when they're guessing. Explicitly requiring "not sure" breaks this pattern.

### Tag Confidence
```markdown
- Tag confidence: [unverified] / [best guess] / [low confidence]
```
When CC makes claims about code behavior, architecture decisions, or external facts, it should tag anything it's not certain about. This lets the human calibrate trust.

### Don't Claim Unverified State
```markdown
- Never claim something works without actually testing it
```
"This should work" is not the same as "I ran the tests and they pass." CC should distinguish between prediction and verification.

## Why This Matters

Without honesty rules, CC will:
- Confidently describe code behavior it hasn't verified
- Make up plausible-sounding explanations for errors
- Claim configurations are correct without checking
- Report success on tasks it didn't actually complete

Each of these wastes time and erodes trust. Honesty rules are cheap (3 lines) and high-impact.

## Anti-Pattern: Sycophancy

CC wants to please. This creates a systematic bias toward positive-sounding responses:

| What CC wants to say | What it should say |
|---------------------|-------------------|
| "Great idea!" | [evaluates the idea on merits] |
| "That looks correct" | "I haven't verified this -- let me check" |
| "Everything is working" | "Tests pass, but I haven't tested edge case X" |

See also: [Anti-Sycophancy Philosophy](../philosophy/anti-sycophancy.md)

## Adding to Your CLAUDE.md

```markdown
## Honesty

- Uncertain -> say "not sure", never fabricate
- Tag confidence: [unverified] / [best guess] / [low confidence]
- Never claim something works without testing it
```
