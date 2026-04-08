# Research -> Plan -> Implement

The 3-stage methodology for complex tasks. Skipping stages is the #1 source of wasted work in Claude Code sessions.

## Why Three Stages?

Without stages, complex tasks unfold like this:
1. CC starts reading code (research)
2. While reading, it starts forming a plan
3. While planning, it starts writing code
4. The code reflects a plan that was formed while still researching
5. New information invalidates the approach
6. Patches on patches until the code is unmaintainable
7. Revert and start over

With stages:
1. Research produces a complete understanding
2. The plan is based on complete information
3. Implementation follows a reviewed plan
4. Changes are intentional, not reactive

## Stage 1: Research

**Goal**: Understand the problem space completely before proposing solutions.

**Process**:
- Deep-read related code, documentation, and materials
- Produce `research.md` with findings
- Human reviews the research before proceeding

**Output**: `research.md` containing:
- Current system behavior
- Relevant code paths and their responsibilities
- Constraints and dependencies
- Edge cases discovered

**Rule**: No code changes during research. The temptation to "fix that while I'm here" is strong. Resist it.

## Stage 2: Plan + Annotation Loop

**Goal**: Produce a concrete, reviewable plan before writing any code.

**Process**:
1. Produce `plan.md` with code snippets, file paths, and TODO items
2. Human reviews and adds annotations (e.g., `[Note: use existing helper here]`)
3. CC processes annotations and updates the plan
4. Repeat 1-N rounds until the plan is approved

**Output**: `plan.md` containing:
- Approach description
- Step-by-step TODO list
- File paths that will be modified
- Code snippets showing the approach
- Acceptance criteria

**Critical rules**:
- **NO code changes during planning** -- planning only
- **plan.md must be a file**, not just conversation -- it's shared mutable state
- Important plans: have a second reviewer (or a second CC session) check it

## Stage 3: Implement (ReAct Loop)

**Goal**: Execute the plan step by step with verification at each step.

**Process**: Each step follows the ReAct cycle:

```
Act -> Observe -> Reflect -> Record -> Next Step
```

- **Act**: Execute the step from plan.md
- **Observe**: Read the actual result -- did it match expectations?
- **Reflect**: If unexpected -> stop, diagnose, decide: continue / adjust plan / revert
- **Record**: Mark step done + write a one-line observation note

**Critical rule**: Wrong direction -> **revert, don't patch**. Patches on mistakes compound into unmaintainable code. It's cheaper to revert 3 steps than to patch around them.

## When to Skip Stages

| Task Size | Approach |
|-----------|----------|
| Trivial (1-5 LOC) | Just do it |
| Small (5-20 LOC, 1 file) | Mental plan, then implement |
| Medium (20-100 LOC, 2-5 files) | Brief written plan, then implement |
| Large (100+ LOC, 5+ files) | Full 3-stage process |
| Critical/irreversible | Full 3-stage + independent review |

## Research-Implementation Separation

- **Same task**: all stages stay in one session for context continuity
- **Different tasks**: each task gets its own session to avoid context pollution
- If research fills too much context: write `research.md` to disk, start a new session that reads it
- Never combine "understand the system" + "build the feature" in one prompt

## Common Mistakes

1. **Skipping research**: "I think I know how this works" -> leads to plans based on wrong assumptions
2. **Research during implementation**: "Let me check how this works" mid-coding -> half-built code based on evolving understanding
3. **Not writing the plan down**: Plans in conversation get modified by later messages without anyone noticing
4. **Not using the annotation loop**: The plan is a conversation, not a monologue -- human annotations catch bad assumptions early
5. **Patching instead of reverting**: 3 patches deep into a wrong approach is much more expensive than reverting
