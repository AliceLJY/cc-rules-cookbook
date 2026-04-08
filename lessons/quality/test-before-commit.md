# Test Before Commit

## The Problem

CC sometimes commits code after writing it without running the test suite. The commit message says "fix: resolve issue X" but the tests were never run. Later, someone pulls the commit and discovers it breaks existing tests -- or worse, it passes tests locally but fails in CI because of environment differences.

## The Rule

```markdown
- Never commit untested code
- Run the project's test suite before every commit
```

The workflow:
1. Write/modify code
2. Run `npm test` (or your project's equivalent)
3. Fix any failures
4. Run lint: `npm run lint`
5. Only then: `git commit`

## Why It Matters

- A commit is a promise that the code works at that point
- Broken commits pollute `git bisect` and make debugging history harder
- "I'll fix the tests later" rarely happens -- later never comes
- CI failures after push waste time and block other developers

## Implementation

Add to your CLAUDE.md `## NEVER` section:

```markdown
- Commit untested code
```

And to `## Verification`:

```markdown
- For code changes: run tests before committing
- Definition of done: tests pass + lint clean + no untracked TODOs
```

You can enforce this with a pre-commit hook:

```bash
#!/bin/sh
# .git/hooks/pre-commit
npm test
if [ $? -ne 0 ]; then
  echo "Tests failed. Commit aborted."
  exit 1
fi
```

Or with a Claude Code hook:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hook": "if echo \"$CC_BASH_COMMAND\" | grep -q 'git commit'; then echo 'Reminder: run tests before committing'; fi"
      }
    ]
  }
}
```
