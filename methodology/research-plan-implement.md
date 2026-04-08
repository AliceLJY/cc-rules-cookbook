# Research -> Plan -> Implement

**A three-stage discipline for complex tasks. Not a suggestion -- a requirement. Skipping stages is the #1 source of wasted work.**

---

## Why Three Stages

Complex tasks fail when research and implementation happen simultaneously. The AI fills its context with exploratory reading, then tries to write code in a polluted context window. The result: confused output that half-understands the problem.

Separation is the fix. Research produces understanding. Planning produces a contract. Implementation executes the contract.

---

## Stage 1: Research

**Goal**: Understand the problem space fully before proposing solutions.

- Deep-read related code, docs, APIs, and constraints
- Produce `research.md` with findings
- Human reviews `research.md` before Stage 2 begins

**Rules**:
- No code changes during research
- No solution proposals yet -- understand first, solve later
- If research fills too much context, write `research.md` to disk and start a fresh session

**Output**: `research.md` containing:
- Current system behavior
- Relevant code paths and file locations
- Constraints and dependencies
- Open questions for the human reviewer

---

## Stage 2: Plan + Annotation Loop

**Goal**: Produce a concrete implementation plan that the human approves before any code is written.

- Write `plan.md` with:
  - Code snippets showing the approach
  - File paths that will be modified
  - TODO list with specific acceptance criteria
  - Risk assessment

### The Annotation Loop

`plan.md` is **shared mutable state** between human and AI.

```
+------------------+          +------------------+
|   CC writes      |          |   Human adds     |
|   plan.md        |--------->|   [Note: ...]    |
|                  |          |   annotations    |
+------------------+          +------------------+
         ^                             |
         |                             |
         +----  CC processes  <--------+
                annotations
                updates plan
```

The loop runs 1-N rounds until the human is satisfied. During this stage:

- **NO code changes** -- planning only
- Annotations use the format `[Note: your comment here]`
- CC processes every annotation and either incorporates it or explains why not
- The plan must be **persisted to a file**, not just discussed in conversation

### Plan File Format

```markdown
# Plan: [Task Name]

## Goal
[One sentence: what are we trying to achieve?]

## Context
[Relevant files, current behavior, constraints]

## Approach
[High-level strategy]

## Steps
- [ ] Step 1: [description] -- files: `path/to/file.ts`
- [ ] Step 2: [description] -- files: `path/to/other.ts`
- [ ] Step 3: [description] -- files: `path/to/test.ts`

## Risks
[What could go wrong? What assumptions are we making?]

## Done When
[Specific, testable criteria]
```

---

## Stage 3: Implement (with ReAct Loop)

**Goal**: Execute `plan.md` step by step with continuous verification.

Each step follows the [ReAct Loop](./react-loop.md):

1. **Act**: Execute the step
2. **Observe**: Read the actual result -- did it match expectations?
3. **Reflect**: If unexpected -> stop, diagnose, decide: continue / adjust plan / revert
4. **Record**: Mark step done + write a one-line `[Observation: ...]` note

**Critical rule**: Wrong direction -> **revert, don't patch**. Patches on top of mistakes compound into unmaintainable code.

---

## Complexity Calibration

Not every task needs all three stages:

| Task Size | Approach |
|-----------|----------|
| Simple (< 20 LOC, single file) | Just do it -- no ceremony needed |
| Medium (20-100 LOC, 2-5 files) | Lightweight plan (5-line outline), then implement |
| Complex (100+ LOC, 5+ files) | Full 3-stage process with written artifacts |
| Critical (production, irreversible) | 3-stage + second reviewer on the plan |

---

## Context Hygiene

- **Same task**: research -> plan -> implement stays in one session (context continuity)
- **Different tasks**: each gets a new session (no context pollution)
- **Context overflow**: write artifacts to disk, start fresh session that reads from disk
- **Parallel tasks**: use git worktrees + separate sessions

The goal is that at any point during implementation, the AI's context contains exactly what it needs and nothing else.

---

## Common Mistakes

1. **"Just build it"**: Jumping straight to implementation. Research fills context with half-understood code, implementation produces half-correct solutions.

2. **Skipping the annotation loop**: The plan looks reasonable so the human says "go." Two hours later, a fundamental misunderstanding surfaces.

3. **Mental-only plans**: Discussing the plan in conversation without writing `plan.md`. The plan drifts, steps get forgotten, scope creeps.

4. **Research in implementation**: Discovering mid-implementation that a dependency works differently than expected. Should have been caught in Stage 1.
