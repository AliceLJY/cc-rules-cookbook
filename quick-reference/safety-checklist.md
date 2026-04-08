# Safety Checklist

**Quick-reference operational rules. Each one exists because it was violated at least once.**

---

## Destructive Operations

- [ ] **No `rm`**: Use `mv <file> ~/.Trash/` instead. `rm` is unrecoverable.
- [ ] **No `git push --force` to main/master**: Without explicit permission, this destroys team history.
- [ ] **No `git reset --hard`** without confirming uncommitted work is saved or disposable.
- [ ] **No destructive operations as first resort**: Always consider safer alternatives first.

## Secrets and Credentials

- [ ] **Never commit `.env`, credentials, API keys, or tokens**.
- [ ] **Never guess** environment variables, secrets, or configuration values.
- [ ] **Never echo/print secrets** to stdout, even in debugging.
- [ ] **Check `.gitignore`** before committing new file types that might contain secrets.

## API and Refactoring Safety

- [ ] **Grep all callers** after changing any function signature or API.
- [ ] **Change one place -> sync all related files**. Don't leave partial updates.
- [ ] **Never commit untested code**. Run the project's test suite first.
- [ ] **When tests fail**: Don't claim "it's not caused by our changes" unless verified on the main branch.

## Git Workflow

- [ ] **PR review fixes**: Append commit on the same branch. Don't open a new PR.
- [ ] **Before creating a PR**: Check `gh pr list` for existing PRs addressing the same issue.
- [ ] **Commit before closing**: Don't leave uncommitted changes at session end.
- [ ] **Bilingual docs**: README.md in English, README_CN.md in the project's secondary language. Don't mix languages in one file.

## Session Discipline

- [ ] **Read tool output completely**: Check actual results, not expected results.
- [ ] **Unexpected error -> stop**: Don't continue executing after something breaks.
- [ ] **After every mistake**: Update CLAUDE.md to prevent recurrence.
- [ ] **Definition of done**: Tests pass + lint clean + no untracked TODOs.

---

## Quick Copy

Minimal safety rules for a new `CLAUDE.md`:

```markdown
## Safety

- Never use `rm` (use `mv` to `~/.Trash/`)
- Never commit untested code
- Never commit secrets (.env, credentials, API keys)
- Change API/add params: grep ALL callers, confirm each
- Change one place: scan and sync ALL related files
- After every mistake: update this file to prevent recurrence
- Unexpected error: stop, diagnose, report — don't continue
```
