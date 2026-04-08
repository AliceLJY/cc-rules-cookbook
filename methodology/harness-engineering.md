# Harness Engineering

**Promoting written rules into automated enforcement. Paper rules are suggestions; code rules are constraints.**

---

## The Core Insight

A rule in `CLAUDE.md` is a suggestion. The AI reads it, usually follows it, sometimes forgets. The forgetting rate increases with context length, task complexity, and number of competing rules.

A hook is a mechanical constraint. It runs automatically, checks conditions programmatically, and blocks violations before they reach the user.

**The progression**: Identify a repeatedly violated rule -> write a hook that enforces it mechanically -> the paper rule becomes a code rule.

---

## The Pattern

```
Incident occurs → Write CLAUDE.md rule → Rule works initially
                                              ↓
                            Rule gets violated again (context overload,
                            competing priorities, model forgets)
                                              ↓
                            Write a hook that enforces the rule
                            mechanically — no AI judgment involved
                                              ↓
                            Rule is now a CODE RULE:
                            violation is impossible, not just discouraged
```

---

## Real Examples

### Example 1: Destructive Delete Prevention

**Paper rule**: "Never use `rm` to delete files. Use `mv` to `~/.Trash/`."

**Violation pattern**: Under time pressure or in complex multi-step tasks, the agent occasionally uses `rm` despite the rule.

**Code rule** (bash pre-command hook):

```bash
#!/bin/bash
# bash-guard.sh — intercepts dangerous commands before execution

COMMAND="$1"

# Block rm usage
if echo "$COMMAND" | grep -qE '\brm\s'; then
  echo "BLOCKED: Use 'mv <file> ~/.Trash/' instead of rm"
  exit 2
fi
```

Result: `rm` is now mechanically impossible, not just discouraged.

### Example 2: Pre-PR Duplicate Check

**Paper rule**: "Before creating a PR, check if an existing PR already addresses the same issue."

**Violation pattern**: Agent runs `gh pr create` without checking.

**Code rule** (bash pre-command hook):

```bash
# Detect gh pr create and remind to check existing PRs
if echo "$COMMAND" | grep -qE 'gh\s+pr\s+create'; then
  echo "REMINDER: Run 'gh pr list' first to check for existing PRs"
  # Allow the command but inject the reminder
fi
```

### Example 3: Dirty Repo Detection at Session End

**Paper rule**: "Commit and push before closing a session."

**Violation pattern**: Sessions end with uncommitted changes, causing drift.

**Code rule** (stop hook):

```bash
#!/bin/bash
# auto-commit.sh — runs at session end

for repo in ~/projects/repo-a ~/projects/repo-b; do
  if [ -d "$repo/.git" ]; then
    cd "$repo"
    if ! git diff --quiet || ! git diff --cached --quiet; then
      echo "WARNING: Uncommitted changes in $repo"
      # Optionally: auto-commit with session summary
    fi
  fi
done
```

### Example 4: ReAct Loop Enforcement

**Paper rule**: "Record observation notes after each step."

**Violation pattern**: Agent executes 10+ tool calls without any observation, losing the ReAct discipline.

**Code rule** (notification hook):

```bash
#!/bin/bash
# nag-reminder.sh — monitors tool call patterns

TOOL_CALLS_WITHOUT_OBSERVATION=$1

if [ "$TOOL_CALLS_WITHOUT_OBSERVATION" -ge 5 ]; then
  echo "You've made $TOOL_CALLS_WITHOUT_OBSERVATION tool calls without recording observations."
  echo "Pause and reflect: did each step produce the expected result?"
fi
```

---

## Hook Types in Claude Code

| Hook Type | When It Runs | Use Case |
|-----------|-------------|----------|
| Pre-command (bash) | Before a shell command executes | Block dangerous commands, inject reminders |
| Post-command (bash) | After a shell command completes | Validate output, check for warnings |
| Notification | On specific events | Monitor behavior patterns, enforce disciplines |
| Stop | When a session ends | Clean up, commit, checkpoint |

---

## Promotion Criteria

Not every rule should become a hook. Promote when:

1. **The rule has been violated 3+ times** despite being documented
2. **The violation has real consequences** (data loss, broken builds, wasted work)
3. **The check is mechanical** (can be expressed as a grep/regex/condition, not a judgment call)
4. **False positives are rare** (the hook won't block legitimate operations)

Don't promote when:

- The rule requires contextual judgment (e.g., "write good commit messages")
- Violations are rare and low-impact
- The hook would produce frequent false positives
- The rule is still evolving and might change

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Rule Enforcement

- Paper rules that get violated repeatedly → promote to hooks
- hooks enforce mechanically — no AI judgment involved
- Three violations of the same rule → write the hook
```

Configure hooks in your Claude Code settings:

```json
{
  "hooks": {
    "bash": [
      {
        "matcher": ".*",
        "hook": ".claude/hooks/bash-guard.sh"
      }
    ],
    "stop": [
      {
        "hook": ".claude/hooks/auto-commit.sh"
      }
    ]
  }
}
```
