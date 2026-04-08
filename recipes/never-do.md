# NEVER Rules

Things CC must never do. Each rule here exists because the violation happened at least once and caused real damage.

## Universal NEVER Rules

### File Safety
```markdown
- Never use `rm` to delete files (use `mv` to `~/.Trash/`)
```
**Why**: `rm` is unrecoverable. One wrong glob pattern and important work vanishes forever. `mv` to Trash gives you a safety net. This rule alone has prevented data loss multiple times.

### Code Quality
```markdown
- Never commit untested code
```
**Why**: Untested commits create a false sense of progress. The bug surfaces later when context is lost, making it 10x harder to fix.

```markdown
- Never force-push to main/master without explicit permission
```
**Why**: Force-push rewrites history and can destroy teammates' work. Even with permission, it should be rare.

### Documentation
```markdown
- Never mix languages in the same documentation file
```
**Why**: Mixed-language docs look unprofessional and are harder to maintain. Use separate files (e.g., `README.md` for English, `README_CN.md` for Chinese) with cross-links at the top.

### Honesty
```markdown
- Never guess at environment variables, secrets, or configuration values
```
**Why**: Wrong config values can connect to production databases, leak credentials, or misconfigure services. Ask rather than guess.

```markdown
- Never claim something works without actually testing it
```
**Why**: "This should work" is the most expensive sentence in software.

### Error Handling
```markdown
- Never continue executing after an unexpected error -- stop, diagnose, report
```
**Why**: Plowing ahead after an error compounds the damage. Each subsequent step operates on a broken foundation.

## How to Add Your Own

The best NEVER rules share these properties:
1. **Born from an incident** -- not theoretical, something actually went wrong
2. **Specific and actionable** -- CC can follow it without interpretation
3. **Includes the alternative** -- not just "don't do X" but "do Y instead"

Template:
```markdown
- Never [bad action] ([do this instead] -- [what goes wrong otherwise])
```

## Common Project-Specific NEVER Rules

These are examples you might adapt:

```markdown
# Database
- Never modify schema without a migration file
- Never run DELETE without a WHERE clause

# Dependencies
- Never install new packages without approval
- Never upgrade major versions without testing

# CI/CD
- Never skip CI checks, even for "small" changes
- Never modify deployment configs without review

# Security
- Never commit .env files or secrets
- Never log sensitive user data
```
