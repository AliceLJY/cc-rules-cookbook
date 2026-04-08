# Borrowed & Adapted Patterns

Patterns borrowed from other projects and adapted for Claude Code workflows. Every entry credits the source, explains the adaptation, and shows where it's used.

**Why this section exists**: Good engineering borrows proven ideas. These aren't copy-pastes -- they're concepts that were studied, filtered for relevance, and reshaped to fit CC workflows. The original authors solved different problems; the adaptations solve ours.

---

| Pattern | Source | Core Idea |
|---------|--------|-----------|
| [Failure Modes -- Five Layers](./failure-modes-five-layers.md) | Huyen Chip | Agent failures form a hierarchy (L1-L5); the layer determines the fix strategy |
| [Report-First](./report-first-pattern.md) | OpenClaw Medical Skills | Build the deliverable silently, deliver only the final report |
| [Boil the Lake](./boil-the-lake.md) | gstack (Garry Tan) | AI makes completeness cheap -- pick simple, then execute fully |
| [Two-Stage Classification](./two-stage-classification.md) | Anthropic Auto Mode | Fast filter + deep analysis; strip the persuasion layer before judging |
| [Measurable Outcome](./measurable-outcome.md) | OpenClaw Medical Skills | Define testable success criteria before starting, self-check after |
