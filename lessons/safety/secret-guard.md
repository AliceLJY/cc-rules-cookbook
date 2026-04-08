# Never Guess Secrets or Config Values

## The Problem

When CC encounters missing environment variables, API keys, or configuration values, it sometimes fills in plausible-looking values instead of asking. This can lead to:
- Connecting to the wrong database (e.g., production instead of development)
- Using invalid API keys that cause silent failures
- Committing placeholder secrets that look real enough to cause confusion

## The Rule

```markdown
- Never guess at environment variables, secrets, or configuration values
```

When a secret, API key, or config value is needed:
1. Check if it's defined in the project's `.env.example` or documentation
2. If not, **ask the human** -- never fill in a guess
3. Never commit actual secrets to git, even in "test" files

## Why It Matters

- Wrong config values can connect to production systems during development
- Placeholder API keys that "look real" get committed and cause confusion
- Security credentials should never be in source control
- The cost of asking is 10 seconds; the cost of a wrong guess can be a security incident

## Implementation

Add to your CLAUDE.md `## NEVER` section:

```markdown
- Guess at environment variables, secrets, or configuration values
```

Also consider:

```markdown
## NEVER
- Commit .env files or any file containing secrets
- Use production URLs/credentials in development configs
```
