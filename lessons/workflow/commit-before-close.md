# Commit and Push Before Closing a Session

## The Problem

Work done in a CC session exists only in the local working tree until it's committed and pushed. If the session ends without committing -- whether by accident, timeout, or context limit -- the work can be lost or orphaned as uncommitted changes that the next session may not notice.

## The Rule

```markdown
- Before ending a session with uncommitted work: commit + push
```

Before closing:
1. `git status` to check for uncommitted changes
2. `git add` and `git commit` with a descriptive message
3. `git push` to the remote
4. If work is incomplete, use a WIP commit: `git commit -m "WIP: [what's done, what's left]"`

## Why It Matters

- Uncommitted changes can be overwritten by the next session
- Other collaborators can't see or build on unpushed work
- Version drift (local state != remote state) causes confusion
- WIP commits with clear messages help the next session pick up where this one left off

## Implementation

Add to your CLAUDE.md `## ALWAYS` section:

```markdown
- Code changes -> commit + push (no version drift)
```

You can also use a Claude Code hook that reminds you to commit before session end:

```json
{
  "hooks": {
    "Stop": [
      {
        "hook": "cd $PROJECT_DIR && if [ -n \"$(git status --porcelain)\" ]; then echo 'WARNING: Uncommitted changes detected. Consider committing before closing.'; fi"
      }
    ]
  }
}
```
