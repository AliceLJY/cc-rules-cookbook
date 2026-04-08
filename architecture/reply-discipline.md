# Reply Discipline

**Systematic attention management that prevents dropped items and context drift.**

---

## The Problem

In long conversations, AI agents develop tunnel vision. They focus intensely on the most recent exchange and lose track of:

- Earlier questions that were asked but not answered
- Checklist items that were acknowledged but not completed
- The main task that spawned a series of side-investigations
- Tool outputs that contained unexpected information

This isn't laziness -- it's a limitation of attention in long contexts. Reply Discipline compensates mechanically.

---

## The Three Rules

### Rule 1: Scan Before Replying

Before composing a response, scan the last 5-10 messages for unhandled items:

- Questions the user asked that haven't been answered
- Requests that were acknowledged ("I'll do that") but not executed
- Checklist items that were discussed but not checked off
- Conditions that were set ("after X, do Y") where X has now happened

**The check**: "Is there anything in the recent conversation that I acknowledged but haven't addressed?"

### Rule 2: Read Tool Output Completely

After running a tool (command, file read, API call), read the **actual result**. Not the expected result -- the actual one.

Common failures:
- Running `npm test` and seeing "14 tests passed" but missing "2 tests failed" at the bottom
- Reading a file and noticing the function signature but missing the TODO comment
- Running `git status` and seeing "nothing to commit" but missing "Your branch is ahead of origin by 3 commits"

**The check**: "Did the tool output contain anything unexpected? If yes, stop and address it."

### Rule 3: Return to Main Line

After handling a side question, tangent, or debugging detour, explicitly return to the main task:

"Back to the main task: the next step is X."

Without this, conversations drift. A question about a dependency leads to investigating the dependency, which leads to updating the dependency, which leads to fixing a breaking change, and the original task is forgotten.

**The check**: "What was I doing before this detour? Am I back on track?"

---

## Practical Examples

### Dropped Question

```
User: "Can you also check if the tests pass? And refactor the date utility."

Agent: *refactors the date utility, forgets to run tests*
```

**With Reply Discipline**: Before declaring the refactor complete, scan for unhandled items. "Can you also check if the tests pass" is unhandled. Run tests.

### Missed Tool Output

```
Agent runs: git push
Output: "rejected: non-fast-forward"
Agent says: "Changes have been pushed successfully."
```

**With Reply Discipline**: Read the actual output. "rejected" is unexpected. Stop, diagnose, report.

### Context Drift

```
User asks: "Implement the user authentication feature"
During implementation, agent notices a bug in the session handler
Agent fixes the session handler bug
Agent keeps investigating session-related code
Original auth feature is forgotten
```

**With Reply Discipline**: After fixing the session bug, explicitly state: "Back to the main task: implementing user authentication. Next step is..."

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Reply Discipline

- Before replying: scan last 5-10 messages for unhandled items
- After tool call: read the actual result; if unexpected -> stop and rethink
- After side-tasks: return to main task explicitly ("Back to main: next step is X")
```
