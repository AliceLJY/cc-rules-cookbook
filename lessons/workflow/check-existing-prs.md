# Check for Existing PRs Before Creating One

## The Problem

CC creates a PR to fix an issue, only to discover that another PR already addresses the same problem. This creates duplicate work, wastes reviewer time, and can cause merge conflicts when both PRs try to modify the same files.

## The Rule

```markdown
- Before creating a PR: check if an existing PR already fixes the same issue
```

Before running `gh pr create`:
1. Run `gh pr list` to see open PRs
2. Check if any existing PR addresses the same issue or touches the same files
3. If a related PR exists, consider contributing to it instead of creating a new one

## Why It Matters

- Duplicate PRs waste reviewer time
- Merge conflicts between competing PRs are painful to resolve
- Contributing to an existing PR is usually faster than starting from scratch
- Shows respect for other contributors' work

## Implementation

Add to your CLAUDE.md `## ALWAYS` section:

```markdown
- Before creating a PR: run `gh pr list` to check for existing PRs on the same issue
```

### Quick Check Pattern

```bash
# Before creating a PR:
gh pr list --state open
gh pr list --search "keyword related to your change"

# If you find a related PR:
gh pr view <number>  # Read what it does
# Then decide: contribute to it, or create a separate PR with a clear explanation of why
```
