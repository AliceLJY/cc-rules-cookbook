# cc-rules-cookbook

**Cross-disciplinary frameworks for Claude Code mastery -- cognitive psychology, risk assessment, ReAct methodology, and engineering discipline. From 500+ sessions.**

<p align="center"><a href="README_CN.md">中文版</a></p>

[![Frameworks](https://img.shields.io/badge/frameworks-5-blue)](./frameworks/)
[![Methodology](https://img.shields.io/badge/methodology-6%20systems-orange)](./methodology/)
[![Architecture](https://img.shields.io/badge/architecture-3%20patterns-green)](./architecture/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## What This Is

This is not a list of "don't do X" rules. It is a cross-disciplinary system that applies cognitive psychology, risk assessment, AI research, and engineering methodology to Claude Code workflows.

The basic rules ("don't use rm", "commit before closing") are in [quick-reference/](./quick-reference/). They matter, but they're not the interesting part. The interesting part is the frameworks -- systematic approaches to the hard problems of human-AI collaboration: getting honest output from a system that wants to please you, filtering signal from noise in code reviews, and enforcing discipline through mechanical constraints instead of willpower.

Everything here was extracted from production use across 500+ sessions. Nothing is theoretical.

---

## The Frameworks

### [Bias Correction Matrix](./frameworks/bias-correction-matrix.md)

Seven cognitive biases that systematically corrupt AI code review, with mechanical countermeasures.

| Bias | What It Does | Countermeasure |
|------|-------------|----------------|
| Sycophancy | Praises code to maintain positive interaction | "Praise is not your job. Finding problems is" |
| Length Bias | Treats verbose code as more thorough | "Penalize verbosity. Concise > lengthy" |
| Authority Bias | Treats confident-sounding code as correct | "Confidence means nothing" |
| Completion Bias | Gives credit for "finished" regardless of quality | "Garbage can be complete" |
| Effort Bias | Softens criticism when effort was clearly high | "Effort is irrelevant. Judge OUTPUT" |
| Recency Bias | Treats newer patterns as inherently better | "New is not better" |
| Familiarity Bias | Treats common patterns as correct | "Common is not correct" |

Default review score: **2/5**. Must argue UP with evidence. 5/5 appears in fewer than 5% of reviews.

Plus six anti-rationalization rules that block escape routes like "it's mostly good" (= partially bad = FAIL) and "they tried hard" (effort is irrelevant).

### [Confidence x Impact Filter](./frameworks/confidence-impact-filter.md)

Quantitative two-dimensional filtering for code review findings. Inspired by risk assessment methodology.

```
Confidence
100 |  REPORT   REPORT   REPORT   REPORT   REPORT
 95 |  report   REPORT   REPORT   REPORT   REPORT
 85 |  ------   report   REPORT   REPORT   REPORT
 75 |  ------   ------   report   REPORT   REPORT
 65 |  ------   ------   ------   report   REPORT
 50 |  ------   ------   ------   ------   report
  0 |  ------   ------   ------   ------   ------
     Trivial    Low     Medium    High    Critical
                        Impact -->
```

Critical findings (security, data loss) get reported at 50% confidence. Trivial findings (style, naming) require 95% confidence. Everything below the threshold is **silently discarded** -- no "minor notes," no "just FYIs."

### [Anti-Sycophancy Protocol](./frameworks/anti-sycophancy.md)

Behavioral psychology applied to AI prompting. The core insight: AI agents tell you what you want to hear because that's what their training rewards.

| Biased Prompt | Neutral Prompt |
|--------------|---------------|
| "Find the bugs in this code" | "Read through the logic, report all findings" |
| "This code has performance issues" | "Analyze this code's performance characteristics" |
| "Is this approach better?" | "Compare approaches on dimensions X, Y, Z" |

For high-stakes decisions: three-agent adversarial verification. Agent A produces, Agent B attacks, Agent C arbitrates.

### [XY Problem Detection](./frameworks/xy-problem-detection.md)

Proactive means-goal mismatch identification. When the user asks for Y (a solution) instead of X (the problem), flag it before implementing the wrong thing.

### [Completion Taxonomy](./frameworks/completion-taxonomy.md)

Four statuses that replace ambiguous "done":

| Status | Meaning |
|--------|---------|
| **DONE** | Tests pass, lint clean, no untracked TODOs |
| **DONE_WITH_CONCERNS** | Complete but has risks -- listed explicitly |
| **BLOCKED** | Cannot proceed -- states what's needed |
| **NEEDS_CONTEXT** | Insufficient information to begin meaningfully |

---

## The Methodology

### [Research -> Plan -> Implement](./methodology/research-plan-implement.md)

Three-stage discipline for complex tasks. Not a suggestion -- a requirement.

```
Research          Plan                Implement
                  (annotation loop)   (ReAct loop)
                  
research.md  -->  plan.md         --> code changes
human reviews     human annotates     step-by-step
                  [Note: ...]         Act/Observe/
                  CC updates          Reflect/Record
                  loop 1-N rounds
                  NO code changes
```

Key innovation: `plan.md` is **shared mutable state**. The human adds `[Note: ...]` annotations directly in the file. The AI processes annotations and updates the plan. This loops until the plan is approved. No code changes happen until the plan is final.

### [ReAct Loop](./methodology/react-loop.md)

Step-level observation from AI research (Yao et al., 2022). After every action: read the actual result, compare against expectations, record findings. Unexpected result -> STOP. Wrong direction -> REVERT, don't patch.

Enforceable via hooks: 5+ tool calls without observation notes triggers an automated reminder.

### [Scope Drift Detection](./methodology/scope-drift-detection.md)

Post-implementation audit. Compare `plan.md` against `git diff --stat`:
1. Changed files not in plan? = scope creep
2. Plan steps without corresponding changes? = missing requirements

Output: `Scope Check: CLEAN / DRIFT DETECTED / REQUIREMENTS MISSING`

### [Task Contracts](./methodology/task-contracts.md)

`{TASK}_CONTRACT.md` defines completion criteria before work begins. "Agent not meeting contract = not done." Different contracts get different sessions. Wrong direction -> revert and restart.

### [Harness Engineering](./methodology/harness-engineering.md)

The concept that separates this system from a list of tips: **paper rules are suggestions; code rules are constraints.**

```
Rule violated repeatedly --> Write a hook --> Rule is now enforced mechanically
```

Examples:
- "Don't use rm" -> hook blocks `rm` with exit 2
- "Commit before closing" -> stop hook detects dirty repos
- "Check existing PRs" -> hook intercepts `gh pr create` and reminds
- "Write observation notes" -> hook detects 5+ tool calls without notes

Three violations of the same rule -> the rule gets promoted from CLAUDE.md to a hook.

### [Skill Evolution](./methodology/skill-evolution.md)

Systematic refinement: repeated workflow -> skill, bad skill -> update, one-off script -> toolbox, behavior pattern -> rule.

---

## Architecture

### [Memory Architecture](./architecture/memory-architecture.md)

Three-dimensional memory (recency x relevance x importance) with dual-write strategy (local files + external service). Compact priorities: architecture decisions > modified files > verification status > open TODOs > tool outputs.

### [Reply Discipline](./architecture/reply-discipline.md)

Three rules: scan for unhandled items before replying, read tool output completely, return to main task after detours.

### [Confidence Tagging](./architecture/confidence-tagging.md)

Honesty taxonomy: `[Verified]` / `[Unverified]` / `[Low Confidence]`. Tag what you know vs. what you're guessing.

---

## Quick Start

Don't start with the safety checklist. Start with the framework that addresses your biggest pain point:

| Your Problem | Start Here |
|-------------|-----------|
| Code reviews are useless -- too polite, too many nitpicks | [Bias Correction Matrix](./frameworks/bias-correction-matrix.md) + [Confidence x Impact Filter](./frameworks/confidence-impact-filter.md) |
| AI agrees with everything I say | [Anti-Sycophancy Protocol](./frameworks/anti-sycophancy.md) |
| Complex tasks produce confused output | [Research -> Plan -> Implement](./methodology/research-plan-implement.md) |
| Rules keep getting forgotten | [Harness Engineering](./methodology/harness-engineering.md) |
| "Done" doesn't mean done | [Task Contracts](./methodology/task-contracts.md) + [Completion Taxonomy](./frameworks/completion-taxonomy.md) |
| Need operational safety basics | [Safety Checklist](./quick-reference/safety-checklist.md) |

For ready-to-use templates: [`templates/CLAUDE.md.template`](./templates/CLAUDE.md.template) and [`templates/workflow.md.template`](./templates/workflow.md.template).

---

## Who This Is For

People who've used Claude Code enough to realize that raw power isn't enough -- you need steering. You've hit the wall where the AI is capable but unreliable, where it follows instructions until it doesn't, where "done" is ambiguous and reviews are sycophantic.

This cookbook is the steering. Not more rules -- better frameworks.

---

## Companion Repos

- [**cc-hooks-gallery**](https://github.com/AliceLJY/cc-hooks-gallery) -- Pre-built hooks for Claude Code automation (the enforcement layer for harness engineering)

---

## License

MIT -- take what works, leave what doesn't.
