# Pattern: Failure Modes -- Five Layers

**Borrowed from**: Huyen Chip -- ["Building A Gen AI Platform"](https://huyenchip.com/2025/01/07/agents.html)
**Adapted for**: Debugging AI agent failures systematically
**Where it lives**: PR-test skills (4 skills use this)

## The Original Idea

Agent failures are not binary (works/broken). They form a hierarchy:

| Layer | Name | Description |
|-------|------|-------------|
| L1 | Invalid Tool | Agent calls a tool that doesn't exist or isn't available |
| L2 | Bad Parameters | Tool exists but parameters are malformed |
| L3 | Wrong Values | Parameters are valid but values are incorrect for the context |
| L4 | Goal Failure | Tool works correctly but doesn't achieve the intended goal |
| L5 | Reflection Error | Agent reflects on failure but draws the wrong conclusion, compounding the problem |

The layers are ordered by difficulty: L1 is obvious and easy to fix, L5 is subtle and dangerous.

## How It's Adapted for Claude Code

When a PR test or agent task fails, instead of "it's broken," classify which layer failed. The layer determines the fix strategy:

- **L1-L2 (Configuration/Schema)** -- The tool definition is wrong, the MCP server isn't connected, or the parameter schema doesn't match. Fix: correct the tool config, restart the server, update the schema.

- **L3 (Context)** -- The tool exists and parameters are valid, but the agent is passing the wrong values because it lacks context. Fix: improve the prompt, add context injection, or provide examples.

- **L4 (Strategy)** -- Everything executes correctly but the approach doesn't solve the problem. Fix: rethink the approach entirely. The tool is fine; the plan is wrong.

- **L5 (Reflection -- most dangerous)** -- The agent detects a failure, "reasons" about it, and takes corrective action that makes things worse. The agent thinks it fixed the problem. This requires human intervention or adversarial review because the agent's self-correction loop is broken.

## Example

A PR-test skill runs but produces a wrong verdict:

1. First check: Did the tool calls succeed? (L1-L2) -- Yes, all tools returned results.
2. Then check: Were the right files examined? (L3) -- Yes, it looked at the changed files.
3. Then check: Did the analysis reach the right conclusion? (L4) -- No. It tested the wrong behavior.
4. Diagnosis: **L4 -- Goal Failure.** The skill's test strategy doesn't match the PR's intent. Fix the skill's approach, not its tool usage.

If instead the skill had noticed the wrong result, re-run the test with different parameters, and produced an even more wrong result -- that's L5. Stop, get a human.

## When to Use

- Any time an AI agent task fails and you need to decide where to focus debugging effort.
- When a fix attempt makes things worse (likely L5 -- stop self-correction, escalate).
- During post-mortems to categorize recurring failure patterns.
