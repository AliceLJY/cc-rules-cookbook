# XY Problem Detection

How to catch and handle requests where the proposed solution doesn't match the actual goal.

## What Is an XY Problem?

The user has Problem X. They think Solution Y will solve it. They ask for help with Y. But Y is the wrong approach to X.

Classic example:
- **X (real problem)**: "I need to get the file extension"
- **Y (proposed solution)**: "How do I get the last 3 characters of a filename?"
- **Why it's wrong**: Extensions can be 2, 3, or 4 characters (`.go`, `.txt`, `.yaml`)
- **Right answer**: Use `path.extname()` or equivalent

## Why AI Assistants Are Bad at This

CC defaults to answering the question as asked. If you say "help me get the last 3 characters," CC will write code to get the last 3 characters -- even if it suspects you actually want the file extension.

This is a combination of:
- **Compliance bias**: trained to fulfill requests, not question them
- **Sycophancy**: questioning a request feels "unhelpful"
- **Literal interpretation**: the safest response is the most literal one

## The Rule

```markdown
- If a request looks like an XY Problem (means and goal are misaligned), point it out before executing
- Not every request needs this -- only flag when you genuinely spot the disconnect
```

The second line is important: this should not become an excuse to question every request. It's a targeted intervention for when CC genuinely detects a mismatch.

## How to Detect

Signs of an XY Problem:
1. The request is oddly specific for a general goal
2. The proposed approach has obvious limitations that suggest a deeper need
3. The user mentions what they're "trying to achieve" separately from what they're "asking for"
4. The solution seems more complex than the problem warrants

## How to Handle

When you detect a potential XY Problem:

```
"I notice you're asking for [Y], which would achieve [limited result].
If your actual goal is [X], a more direct approach would be [Z].
Want me to proceed with [Y] as asked, or try [Z] instead?"
```

Key principles:
- Explain why you think there's a mismatch
- Offer the alternative
- Let the human decide -- they may have context you don't
- If they confirm Y, do Y without further pushback

## Examples in CC Contexts

### Example 1: Optimization
- **Request**: "Optimize this SQL query, it's slow"
- **XY Problem**: The query is fine, but it's missing an index
- **Better**: "Before optimizing the query itself, I'd check if the table has appropriate indexes for the WHERE clause columns"

### Example 2: Architecture
- **Request**: "Add a caching layer to speed up the API"
- **XY Problem**: The API is slow because it makes 50 sequential database calls that could be batched
- **Better**: "The bottleneck appears to be sequential queries, not lack of caching. Batching the queries might give a bigger speedup with less complexity"

### Example 3: Debugging
- **Request**: "The deploy script fails, add a retry loop"
- **XY Problem**: The script fails because of a missing environment variable
- **Better**: "The script fails at line 42 because $DB_HOST is unset. Fixing the env var is more reliable than retrying"

## Adding to Your CLAUDE.md

```markdown
## Direction Check
- If a request looks like an XY Problem (means and goal misaligned), point it out before executing
- Not every request needs this -- only when genuinely detected
- Offer the alternative, but let the human decide
```
