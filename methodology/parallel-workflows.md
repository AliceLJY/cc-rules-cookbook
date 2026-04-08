# Parallel Workflows

How to handle multiple tasks efficiently without context pollution.

## The Problem

When you work on unrelated tasks in a single CC session:
- Context from Task A bleeds into Task B decisions
- The session becomes unwieldy and slow
- A mistake in one task can disrupt the other
- Compacting (context cleanup) risks losing important context from either task

## The Solution: One Task, One Session

### Git Worktrees for Parallel Development

```bash
# Create worktrees for parallel tasks
git worktree add ../project-feature-auth feature/auth
git worktree add ../project-feature-search feature/search

# Run separate CC sessions in each worktree
# Session 1: cd ../project-feature-auth && claude
# Session 2: cd ../project-feature-search && claude
```

Each session has:
- Its own working directory
- Its own git branch
- Its own context window
- No interference with the other

### When to Parallelize

| Situation | Approach |
|-----------|----------|
| Two features touching different files | Parallel sessions |
| Feature + its tests | Same session (same context needed) |
| Bug fix + unrelated feature | Parallel sessions |
| Research + implementation (same task) | Same session |
| Research + implementation (different tasks) | Parallel sessions |

## Subagents for Subtask Parallelism

Within a single session, use subagents for independent subtasks:

```
Main session: coordinating the overall plan
  |
  +-- Subagent 1: research component A
  +-- Subagent 2: research component B
  +-- Subagent 3: run tests on component C
```

Rules for subagents:
- Don't downgrade model quality -- match the parent's capability
- Use lightweight "explore" agents for pure search/read tasks
- Subagent results come back to the main context, so keep them focused

## Context Contamination Prevention

### Symptoms
- "Let me also look at..." (drifting to unrelated code)
- Session context growing beyond 50% without clear progress
- Mixing terminology from different tasks
- Can't remember what the original goal was

### Prevention
1. Define the task scope before starting (use a [Task Contract](./task-contract.md))
2. If a second task emerges, note it and handle it in a new session
3. When research fills too much context, persist findings to a file and start fresh

## Adding to Your Workflow

```markdown
## Parallel Work
- Independent tasks -> separate sessions (git worktrees if same repo)
- Avoid mixing unrelated tasks in one session
- Subagents for independent subtasks within a session
- Context getting bloated? -> persist to file, start fresh
```
