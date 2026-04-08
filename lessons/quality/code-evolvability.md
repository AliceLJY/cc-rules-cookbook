# Design Code for Evolvability

## The Problem

CC tends to write code that "works right now" without considering what happens when the next developer (or the next CC session) needs to extend it. Common symptoms:

- Logic buried deep inside initialization functions where it can't be found
- Inline monkey-patches that work but have no test coverage
- Tightly coupled code that can't be modified without breaking unrelated features

In one incident, an inline patch was hidden inside a large initialization function. It worked, but when a related feature needed to be added later, the developer had no idea the patch existed and accidentally broke it. Extracting it into a separate module with tests made the extension path clear.

## The Rule

```markdown
- Before submitting code, ask: can the next developer extend this safely?
```

For every new feature or fix:
1. Is the logic in a discoverable location? (Not buried in initialization)
2. Is it independently testable? (Has its own test file or test cases)
3. Is the extension path clear? (Could someone add a related feature without reading every line?)

If logic is hidden inside a large function and can't be tested independently, extract it into its own module first.

## Why It Matters

- Code is read and modified far more often than it's written
- Hidden logic causes surprise breakage during seemingly unrelated changes
- Test coverage protects against accidental regressions during extension
- Clear module boundaries let developers work on parts of the system without understanding all of it

## Implementation

Add to your CLAUDE.md as a review checkpoint:

```markdown
## Quality
- Before submitting: can the next developer extend this safely?
- Hidden logic -> extract to module + add tests before merging
```
