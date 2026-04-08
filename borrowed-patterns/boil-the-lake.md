# Pattern: Boil the Lake

**Borrowed from**: [gstack](https://github.com/garrytan/gstack) (Garry Tan / YC CEO, 50K+ ⭐)
**Adapted for**: Deciding scope and completeness of AI-assisted implementations
**Where it lives**: Planning phase of any complex task

## The Original Idea

Garry Tan's core insight: **AI makes the marginal cost of a complete solution approach zero.** Therefore, the old engineering trade-off of "do we build the full thing or cut corners?" no longer applies the same way.

The metaphor: if boiling a lake used to cost a fortune, and now it costs pennies, you should boil the lake.

Two rules:

1. **Pick the simplest viable approach** -- don't over-architect
2. **Once picked, execute it completely** -- no half-measures, no "we'll add tests later," no TODO stubs

## How It's Adapted for Claude Code

This principle applies at the plan stage, not the research stage:

### During Research: Explore Widely

Consider multiple approaches, compare trade-offs, understand the landscape. This is where simplicity gets *chosen*, not assumed.

### During Planning: Pick Simple, Plan Complete

The plan should describe the simplest approach that solves the problem -- but it should describe it *completely*. Every file that needs changing, every test that needs writing, every edge case that needs handling.

### During Implementation: No Shortcuts

Execute the plan fully. Specific implications:

| Shortcut | Boil-the-Lake Version |
|----------|----------------------|
| "Tests can come later" | Tests are in the plan and get written now |
| "We'll handle edge cases in v2" | Edge cases identified in plan are handled now |
| "Just a quick prototype" | If it's worth building, build it properly |
| "Docs aren't urgent" | If the plan includes docs, write them now |
| "Let's skip lint for speed" | Lint is part of done. Run it. |

### The Boundary

Boil the Lake does NOT mean:
- Build everything conceivable (that's scope creep)
- Add features not in the plan (that's gold-plating)
- Refuse to ship until perfect (that's perfectionism)

It means: **what you decided to build, build completely.** The plan is the scope. Execute all of it, none beyond it.

## Interaction with Other Patterns

- **Scope Drift Detection** catches when you're boiling the *wrong* lake (doing more than planned)
- **Task Contracts** define what "complete" means before you start
- **Completion Taxonomy** distinguishes DONE (fully boiled) from DONE_WITH_CONCERNS (boiled but found something unexpected)

## When to Use

- Any time you're tempted to say "good enough for now"
- When the plan has 10 steps and step 8 is "write tests" -- don't skip step 8
- When the cost of completeness is low (AI-assisted) but the cost of incompleteness is high (technical debt)
