# XY Problem Detection

**Proactive means-goal mismatch identification. Catching wrong solutions before they're implemented.**

---

## The Problem

The XY Problem: a user wants to accomplish X. They think Y is the solution. They ask for help with Y instead of X. The AI dutifully implements Y, which either doesn't solve X or solves it in a needlessly complex way.

AI agents make this worse because they're compliant. Ask for Y, and you'll get Y -- fast, thoroughly, and completely wrong.

---

## The Detection Pattern

When processing a request, check:

1. **Is the user asking for a specific solution (Y) rather than describing a problem (X)?**
2. **Does the solution seem like an unusual or indirect way to achieve the likely goal?**
3. **Would understanding the underlying goal change the approach?**

If yes to any of these: **state the observation before executing.**

```
"It sounds like you want to achieve [X]. You're asking me to do [Y].
Before I proceed -- is [Y] definitely the right approach, or would
[alternative] be a more direct path to [X]?"
```

---

## Examples

### Example 1: Parsing HTML with Regex

**Request**: "Write a regex to extract all `<a>` tags with class 'nav-link' from this HTML"

**XY Detection**: The user wants to extract navigation links (X). They're asking for regex on HTML (Y). An HTML parser would be more robust.

**Response**: "I can write that regex, but HTML parsing with regex is fragile -- nested tags, attribute ordering, and encoding will break it. Would an HTML parser (like BeautifulSoup or cheerio) work for your use case? If regex is intentional (e.g., for a quick grep), I'll proceed."

### Example 2: Circular Workaround

**Request**: "Add a 2-second sleep between the API call and the database write"

**XY Detection**: The user is dealing with a race condition (X). They want a sleep as a fix (Y). The sleep might mask the problem without solving it.

**Response**: "Adding a sleep suggests there's a timing dependency between the API response and the DB write. Would it be better to make the DB write wait on the API response explicitly (callback, await, or queue)? If the sleep is a deliberate temporary measure, I'll add it."

### Example 3: Over-Engineering

**Request**: "Set up a Kubernetes cluster to deploy this single-page application"

**XY Detection**: The user wants to deploy a web app (X). They're asking for Kubernetes (Y), which is substantial infrastructure for a single SPA.

**Response**: "Kubernetes is powerful but might be overkill for a single SPA. Alternatives: static hosting (Vercel, Netlify, S3+CloudFront), or a simple container on a single VM. Do you have specific scaling/orchestration needs that require Kubernetes, or is the goal just reliable deployment?"

---

## When NOT to Flag

- The user has clearly thought about it and chosen the approach deliberately
- The request is straightforward with no apparent means-goal mismatch
- You've already flagged it once and the user confirmed their intent
- The cost of the "wrong" approach is trivially low

**This is not a gatekeeping mechanism.** It's a brief sanity check. If in doubt, execute the request and mention the alternative afterward.

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Direction Check

- If a request looks like an XY Problem (means and goal are misaligned), point it out before executing
- Not every request needs this — only flag when you genuinely spot the disconnect
- After flagging once, respect the user's decision
```

---

## Why This Works

AI agents are optimized for compliance. "Do X" produces X, regardless of whether X is the right thing. The XY Problem check inserts a moment of reflection between receiving a request and executing it.

This costs 10 seconds of clarification and saves hours of implementing the wrong solution.
