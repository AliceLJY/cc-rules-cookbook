# ALWAYS Rules

Things CC must always do. These are positive habits that prevent entire categories of bugs.

## Universal ALWAYS Rules

### Sync Everything
```markdown
- Change one place -> scan and sync ALL related files
```
**Why**: Partial updates are the #1 source of "it works in development but breaks in production." When you change a type definition, function signature, or config value, there are usually 3-5 other places that need updating.

### Grep All Callers
```markdown
- Change API/add params -> grep ALL callers, confirm each is updated
```
**Why**: The most common code review failure is changing a function signature but only updating 2 of 5 call sites. `grep -r "function_name"` takes 2 seconds and prevents hours of debugging. See [grep-all-callers lesson](../lessons/workflow/grep-all-callers.md).

### PR Workflow
```markdown
- PR review fixes -> append commit on same PR branch, don't open new PR
```
**Why**: Opening a new PR for review fixes loses the review history and discussion context. Always push to the same branch.

```markdown
- Before creating a PR: check if an existing PR already addresses the same issue
```
**Why**: Duplicate PRs waste reviewer time and create merge conflicts.

### Test Before Ship
```markdown
- Code changes -> run tests, then commit
```
**Why**: The commit is the promise that the code works. Breaking that promise creates technical debt that compounds over time.

## How to Add Your Own

Good ALWAYS rules are:
1. **Observable** -- you can verify compliance by looking at the output
2. **Cheap** -- they take seconds, not minutes
3. **High leverage** -- they prevent expensive problems

Template:
```markdown
- [Trigger condition] -> [required action] ([why])
```

## Common Project-Specific ALWAYS Rules

```markdown
# Documentation
- Update CHANGELOG.md for user-facing changes
- Add JSDoc/docstring to all exported/public functions

# Testing
- New feature = new test file
- Bug fix = regression test that reproduces the bug first

# Git
- Write descriptive commit messages (what + why, not just "fix bug")
- Keep commits atomic -- one logical change per commit

# Communication
- After completing a task, summarize what was done and any concerns
- When blocked, explain what you tried and what didn't work
```
