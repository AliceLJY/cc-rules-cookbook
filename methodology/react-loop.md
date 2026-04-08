# The ReAct Loop

**Act -> Observe -> Reflect -> Record**

A step-level verification loop that catches problems early, when they're cheap to fix.

## The Loop

```
+-------+     +---------+     +---------+     +--------+
|  Act  | --> | Observe | --> | Reflect | --> | Record |
+-------+     +---------+     +---------+     +--------+
    ^                                              |
    |                                              |
    +---------- next step <------------------------+
```

### Act
Execute the current step from the plan. Run the command, write the code, make the change.

### Observe
**Read the actual result.** Not what you expected to happen -- what actually happened.

This is where most failures occur. CC has a strong tendency to:
- Assume commands succeeded without reading output
- Skim error messages and continue anyway
- Pattern-match on partial output ("I see `200 OK` so it worked" without reading the response body)

Rule: Read the entire output. Every line.

### Reflect
Compare the result to expectations:
- **Matches expectations**: Continue to next step
- **Minor deviation**: Note it, decide if it matters, continue cautiously
- **Unexpected result**: **STOP**. Do not proceed to the next step.

When stopped:
1. Diagnose: What happened? Why is it different from expected?
2. Decide: Is the plan still valid? Does it need adjustment?
3. Act: Continue / adjust plan / **revert**

### Record
Write a one-line observation note on the plan step:

```markdown
- [x] Step 3: Add validation to user input
  [Observation: validation works, but discovered that input is pre-sanitized by middleware -- validation is redundant but harmless, keeping for defense-in-depth]
```

This creates a trail that future sessions (or future you) can use to understand what happened.

## Why Each Step Matters

| Skip this... | ...and you get this |
|--------------|-------------------|
| Act | Nothing gets done (obvious) |
| Observe | Building on broken foundations |
| Reflect | Repeating the same mistakes |
| Record | Next session has no context |

## Real-World Example

```markdown
Plan: Add caching to the search endpoint

- [x] Step 1: Add Redis client initialization
  [Observation: connected successfully, using default port 6379]

- [x] Step 2: Add cache-check before database query
  [Observation: cache miss returns null as expected, cache hit returns stale string -- need JSON.parse]

- [x] Step 3: Add cache-write after database query
  [Observation: UNEXPECTED -- Redis SET returns OK but GET returns truncated value.
   Diagnosed: value exceeds default maxmemory. Adjusted plan to add TTL and check memory config]

- [x] Step 4: (adjusted) Configure Redis maxmemory and add TTL
  [Observation: TTL of 300s working, memory stays within bounds]

- [x] Step 5: Add cache invalidation on write
  [Observation: invalidation works, verified with manual test]
```

Notice how Step 3's observation changed the plan. Without the Observe step, the truncated cache values would have shipped to production.

## Adding to Your Workflow

Add to your `workflow.md` or `CLAUDE.md`:

```markdown
## Implementation
- Execute plan step by step using the ReAct loop: Act -> Observe -> Reflect -> Record
- After each step: read the actual result completely
- Unexpected result -> STOP, diagnose, decide: continue / adjust / revert
- Wrong direction -> revert, don't patch
```
