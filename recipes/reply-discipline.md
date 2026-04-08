# Reply Discipline

Rules that govern how CC communicates and maintains focus during conversations.

## Core Rules

### Check for Unhandled Items
```markdown
- Before replying: scan last 5-10 messages for unhandled items (questions, requests, checklists)
```
**Why**: In long conversations, CC tends to "forget" earlier requests as new ones come in. Scanning backward prevents dropped items.

### Read Tool Output Completely
```markdown
- After tool call: read the actual result completely; if unexpected -> stop and rethink
```
**Why**: CC has a tendency to assume tool calls succeed and continue without checking. This leads to building on broken foundations. Always read the output and verify it matches expectations.

### Stay on the Main Thread
```markdown
- After handling a side question, explicitly return to the main task
```
**Why**: Conversations naturally branch. Without this rule, CC gets lost in tangents and never returns to the primary task. The explicit "Back to the main task: next step is X" keeps things on track.

## Anti-Patterns

### The Optimistic Continue
```
CC: *runs a command*
CC: "Great, that worked! Now let me..."  <-- didn't actually read the output
```
**Fix**: Always read the output. "Great, that worked" should only appear after verifying the output contains the expected result.

### The Dropped Thread
```
Human: "Fix bug X and also update the docs"
CC: *fixes bug X*
CC: "Done!"  <-- forgot about the docs
```
**Fix**: Before saying "Done!", scan back through the conversation for all requested items.

### The Endless Tangent
```
Human: "Deploy the app"
CC: *notices a lint warning during deploy*
CC: *starts fixing lint issues*
CC: *refactors the linted code*
CC: *adds tests for the refactored code*
...  <-- the app was never deployed
```
**Fix**: Handle the tangent minimally (note the lint warning), then return to main task (deploy the app).

## Adding to Your CLAUDE.md

```markdown
## Reply Discipline

- Before replying: scan last 5-10 messages for unhandled items
- After tool call: read the actual result; if unexpected -> stop and rethink
- After handling a side issue: return to the main task explicitly
```
