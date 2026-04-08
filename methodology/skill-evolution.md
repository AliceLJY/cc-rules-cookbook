# Skill Evolution

**Systematic identification and encapsulation of recurring patterns into reusable components.**

---

## The Problem

Over hundreds of sessions, patterns emerge:

- The same 5-step workflow gets executed repeatedly
- A useful one-off script keeps being recreated from scratch
- Behavior corrections accumulate but aren't formalized
- Skills that seemed useful turn out to have poor ergonomics

Without systematic evolution, these patterns remain as tribal knowledge in CLAUDE.md or, worse, in the human's memory.

---

## The Evolution Checks

At session checkpoints (when saving progress or ending a session), evaluate:

### 1. Workflow -> Skill

**Trigger**: A 3+ step workflow has been executed manually more than twice.

**Action**: Encapsulate it as a Claude Code Skill with proper documentation, input validation, and error handling.

**Example**: "Fetch a URL, extract the content, translate it, format it as markdown" -- if done 3 times, it should be a skill, not a manual sequence.

### 2. Bad Skill -> Updated Skill

**Trigger**: A skill exists but the experience of using it is consistently poor (wrong defaults, missing options, confusing output).

**Action**: Update the skill. If the skill has stable parts and unstable parts, split the stable parts into a standalone CLI tool and keep only the dynamic parts as a skill.

### 3. One-Off Script -> Toolbox

**Trigger**: A script was written for a "one-time" task but has been needed again.

**Action**: Move it to a shared toolbox directory, add documentation, handle edge cases, make it reusable.

### 4. Behavior Pattern -> Rule

**Trigger**: A recurring behavior pattern (positive or negative) is observed but not documented in CLAUDE.md.

**Action**: Write the rule. If it's a correction, add it to the feedback index. If it's a workflow, add it to methodology.

### 5. Correction -> Contract

**Trigger**: A behavior correction has been given but is not yet reflected in the relevant section of CLAUDE.md or the project contract.

**Action**: Add it to the appropriate location. Corrections that stay only in conversation history will be forgotten in the next session.

---

## Evolution Checklist

Run this at session checkpoints:

```markdown
## Skill Evolution Check

- [ ] Any 3+ step workflow repeated? → Encapsulate as Skill
- [ ] Any Skill with poor UX? → Update or split
- [ ] Any "one-off" script used twice? → Formalize into toolbox
- [ ] Any recurring behavior not in CLAUDE.md? → Write the rule
- [ ] Any correction not in contract? → Add to relevant section
```

---

## Skill Lifecycle

```
Ad-hoc commands → Repeated pattern → Encapsulated skill
                                          ↓
                              Feedback from usage
                                          ↓
                              Refined skill (better defaults,
                              error handling, documentation)
                                          ↓
                              Stable parts → CLI tool
                              Dynamic parts → remain as skill
```

---

## Anti-Patterns

### The Premature Skill

Creating a skill after using a pattern once. The pattern isn't proven yet. Wait for 3+ repetitions to confirm it's genuinely recurring.

### The Frozen Skill

A skill that was written once and never updated despite consistent friction. Skills are living artifacts -- they should evolve with usage.

### The Mega-Skill

A skill that tries to do everything. Large skills are hard to maintain, hard to test, and hard to understand. Split into focused, composable skills.

### The Undocumented Skill

A skill with no description of what it does, when to use it, or what inputs it expects. Future sessions won't know it exists or how to use it.

---

## Implementation

Add to your `CLAUDE.md`:

```markdown
## Skill Evolution

During checkpoint, evaluate:
- Repeated 3+ step workflow? → Encapsulate as Skill
- Used a Skill but bad experience? → Update or split
- One-off script that recurred? → Formalize into toolbox
- Recurring behavior not in CLAUDE.md? → Write the rule
- Correction not yet in contract? → Add to relevant section
```
