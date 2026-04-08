# PR Review: Fix on the Same Branch

## The Problem

When a PR receives review feedback, CC sometimes creates a new branch and opens a new PR for the fixes. This loses the review history, discussion context, and makes it harder for reviewers to track what changed between rounds.

A single PR might go through 3-4 rounds of review. Each round should be a new commit on the same branch, not a new PR.

## The Rule

```markdown
- PR review fixes -> append commit on same PR branch, don't open new PR
```

When addressing review feedback:
1. Checkout the original PR branch
2. Make the requested changes
3. Commit with a message referencing the review (e.g., "address review: add validation for edge case")
4. Push to the same branch
5. Reply to the reviewer, referencing the new commit hash

Never `gh pr create`. Never create a new branch.

## Why It Matters

- Review history and discussion threads stay connected to the PR
- Reviewers can see incremental diffs between review rounds
- CI runs on the updated PR automatically
- The PR merge preserves the full conversation

## Implementation

Add to your CLAUDE.md `## ALWAYS` section:

```markdown
- PR review fixes -> append commit on same PR branch, don't open new PR
```

### Workflow

```bash
# After receiving review feedback:
git checkout feature/my-pr-branch
# Make changes...
git add -A
git commit -m "address review: fix edge case handling"
git push origin feature/my-pr-branch
# Reply to reviewer with commit hash
```
