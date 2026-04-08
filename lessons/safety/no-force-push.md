# Never Force-Push to Shared Branches

## The Problem

`git push --force` rewrites remote history. If another developer (or another CC session) has based work on the existing history, force-push destroys their reference point. They'll face merge conflicts, lost commits, or silently overwritten changes.

CC sometimes reaches for `--force` when a push fails due to diverged history, treating it as the quick fix. This is almost always the wrong solution.

## The Rule

```markdown
- Never force-push to main/master or shared branches without explicit human permission
```

When a push fails:
1. `git pull --rebase` to catch up with remote
2. Resolve any conflicts
3. Push normally
4. If that doesn't work, **ask the human** before using `--force`

## Why It Matters

- Force-push destroys commits that may contain hours of work
- Other developers lose their reference point without warning
- In CI/CD pipelines, force-push can trigger unexpected deployments
- The "fix" is often worse than the problem it's solving

## Implementation

Add to your CLAUDE.md `## NEVER` section:

```markdown
- Force-push to main/master without explicit permission
```

For extra safety, you can use git hooks to prevent force-push:

```bash
# .git/hooks/pre-push
#!/bin/sh
if echo "$1" | grep -qE '(main|master)'; then
  if grep -q '\-\-force' <<< "$*"; then
    echo "ERROR: Force-push to $1 is blocked. Use a regular push or ask for permission."
    exit 1
  fi
fi
```
