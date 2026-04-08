# Separate Languages in Documentation

## The Problem

When a project needs documentation in multiple languages, the temptation is to put both in a single file -- English paragraph followed by Chinese translation, or using blockquotes for the second language. This looks unprofessional, makes both versions harder to read, and creates maintenance headaches when one language gets updated but the other doesn't.

## The Rule

```markdown
- Never mix languages in the same documentation file
- Use README.md for English, README_CN.md (or README_[LANG].md) for other languages
```

Each language gets its own complete file:
- `README.md` -- English (default)
- `README_CN.md` -- Chinese
- `README_JP.md` -- Japanese
- etc.

Each file should have a language switcher link at the top:

```markdown
<!-- In README.md -->
[中文版](./README_CN.md)

<!-- In README_CN.md -->
[English](./README.md)
```

## Why It Matters

- Mixed-language files look unprofessional to readers of either language
- Maintenance is harder: changes to one language often miss the other
- Search engines index mixed-language content poorly
- Contributors who speak only one language can't easily edit the file

## Implementation

Add to your CLAUDE.md `## NEVER` section:

```markdown
- Mix languages in the same documentation file (separate into README.md and README_CN.md)
```

And to `## ALWAYS`:

```markdown
- Documentation: README.md in English, README_[LANG].md for other languages, with cross-links
```
