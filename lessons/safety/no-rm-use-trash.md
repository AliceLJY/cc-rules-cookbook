# Never Use `rm` -- Use Trash

## The Problem

During a session, CC needed to clean up a file and ran `rm` on it. The file turned out to be a working version that had been carefully edited in another session. Because `rm` is unrecoverable, the work was permanently lost. There was no backup, no git commit, no way to get it back.

This wasn't a one-time accident. AI assistants reach for `rm` by default because it's the "standard" way to delete files. But in an AI-assisted workflow where multiple sessions may be active and files change rapidly, `rm` is a loaded gun.

## The Rule

```markdown
- Never use `rm` to delete files (use `mv` to `~/.Trash/`)
```

For any file deletion -- temporary files, backups, user-requested deletions -- always use:
```bash
mv "$FILE" ~/.Trash/
```

No exceptions.

## Why It Matters

- `rm` is **permanently irreversible** -- there is no undo
- AI assistants make mistakes with glob patterns (`rm *.log` can become `rm *` with one typo)
- Multiple CC sessions may be editing the same files concurrently
- The cost of `mv` to Trash is near zero; the cost of accidental `rm` can be hours of lost work
- Trash can be emptied later once you're sure the files aren't needed

## Implementation

Add to your CLAUDE.md `## NEVER` section:

```markdown
- Use `rm` to delete files (use `mv` to `~/.Trash/`)
```

You can also enforce this with a Claude Code hook that intercepts `rm` commands:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hook": "if echo \"$CC_BASH_COMMAND\" | grep -q '\\brm\\b'; then echo 'BLOCK: Use mv to ~/.Trash/ instead of rm'; exit 1; fi"
      }
    ]
  }
}
```
