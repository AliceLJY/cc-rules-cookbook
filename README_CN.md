# cc-rules-cookbook

**500+ 场实战沉淀的 Claude Code 规则手册 -- CC 高手的捷径**

<p align="center"><a href="README.md">🇬🇧 English</a></p>

[![Rules](https://img.shields.io/badge/rules-30%2B-blue)](./recipes/)
[![Lessons](https://img.shields.io/badge/lessons-34-green)](./lessons/)
[![Methodology](https://img.shields.io/badge/methodology-3%20stages-orange)](./methodology/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## 为什么规则这么重要

没有规则的 Claude Code 就像一辆没方向盘的跑车 -- 动力惊人，但你根本控不住方向。500+ 场实战下来，我们的结论是：

- **不写规则的 CC** 输出不稳定，偶尔搞破坏，你得全程盯着
- **规则写得好的 CC** 是个靠谱搭档，按你的流程走，能自己发现错误，还越用越顺手
- **最好的规则来自翻过的车** -- 这本手册里的每一条，都是真实踩坑后写下的

CC 用得顺不顺，差别几乎全在你的 `CLAUDE.md` 写得好不好。

---

## 快速上手

在项目根目录创建 `CLAUDE.md`，写上这 10 行，效果立竿见影：

```markdown
# Project Rules

## Execution
- Multi-step tasks: auto-confirm, stop only on error or need input
- After every mistake: update CLAUDE.md to prevent recurrence

## Honesty
- Uncertain: say "not sure", never fabricate

## Safety
- Never use `rm` to delete files (use `mv` to `~/.Trash/`)
- Never commit untested code
- Change API/add params: grep ALL callers and confirm each

## Verification
- Run tests/lint before declaring done
- After tool call: read the actual result; if unexpected, stop and rethink
```

然后在该目录运行 `claude` 就行了 -- CC 会自动读取 `CLAUDE.md`。

完整带注释的模板看这里：[`templates/CLAUDE.md.template`](./templates/CLAUDE.md.template)。

---

## 目录结构

| 目录 | 内容 | 从这里开始 |
|------|------|-----------|
| [`templates/`](./templates/) | 开箱即用的 CLAUDE.md 和工作流模板 | [CLAUDE.md.template](./templates/CLAUDE.md.template) |
| [`recipes/`](./recipes/) | 按类别整理的规则集（执行、诚实、安全等） | [execution-principles.md](./recipes/execution-principles.md) |
| [`lessons/`](./lessons/) | 34 条从真实事故中提炼的经验 | [Lessons Index](./lessons/README.md) |
| [`methodology/`](./methodology/) | 三阶段工作流体系 | [research-plan-implement.md](./methodology/research-plan-implement.md) |
| [`philosophy/`](./philosophy/) | AI 协作的深层思考 | [anti-sycophancy.md](./philosophy/anti-sycophancy.md) |

---

## 三阶段工作法

复杂任务必须走三个阶段。跳过阶段是返工的头号原因。

```
+-------------------+     +-------------------+     +-------------------+
|                   |     |                   |     |                   |
|    1. RESEARCH    |---->|     2. PLAN       |---->|   3. IMPLEMENT    |
|                   |     |                   |     |                   |
| - Read code/docs  |     | - Write plan.md   |     | - Execute plan    |
| - Produce         |     | - Code snippets   |     | - Step by step    |
|   research.md     |     | - File paths      |     |                   |
| - Human reviews   |     | - TODO list       |     |  +-------------+ |
|                   |     | - Annotation loop |     |  | ReAct Loop  | |
|                   |     |   (1-N rounds)    |     |  |             | |
|                   |     | - NO code changes |     |  | Act         | |
|                   |     |                   |     |  | Observe     | |
|                   |     |                   |     |  | Reflect     | |
|                   |     |                   |     |  | Record      | |
|                   |     |                   |     |  +-------------+ |
+-------------------+     +-------------------+     +-------------------+
```

**核心原则**：调研和实施严格分开，不要混在一起。一旦混了，上下文会被污染，输出质量直线下降。

详细指南见 [`methodology/`](./methodology/) 目录。

---

## 十大经验

500+ 场实战中影响最大的 10 条规则：

| # | 经验 | 分类 | 效果 |
|---|------|------|------|
| 1 | [别用 `rm` -- 用 Trash](./lessons/safety/no-rm-use-trash.md) | 安全 | 多次救回误删文件 |
| 2 | [改 API 后 grep 所有调用方](./lessons/workflow/grep-all-callers.md) | 工作流 | 杜绝了改一半的重构 |
| 3 | [先问有没有必要再动手](./lessons/communication/question-before-execute.md) | 沟通 | 省下大量无效 token |
| 4 | [先测试再 commit](./lessons/quality/test-before-commit.md) | 质量 | 不再把坏代码推上去 |
| 5 | [PR 修改：追加 commit，别开新 PR](./lessons/workflow/pr-review-workflow.md) | 工作流 | 保留完整 review 历史 |
| 6 | [为可演进而设计](./lessons/quality/code-evolvability.md) | 质量 | 代码架构更健康 |
| 7 | [README 中英文分文件写](./lessons/quality/readme-bilingual.md) | 质量 | 文档更专业 |
| 8 | [提 PR 前先查有没有现成的](./lessons/workflow/check-existing-prs.md) | 工作流 | 避免重复劳动 |
| 9 | [用中性提示词](./lessons/communication/neutral-prompts.md) | 沟通 | Code review 更准确 |
| 10 | [把工具输出读完整](./recipes/reply-discipline.md) | 纪律 | 更早发现问题 |

---

## 规则写作指南

### 怎样写出有效的 CLAUDE.md 规则

**该这样做：**
- 从真实事故中提炼规则，不要凭空想象
- 每条规则控制在 1-2 行
- 写清楚"为什么" -- CC 理解原因后执行力更强
- 用祈使语气："Never use rm" 而不是 "rm should not be used"
- 相关规则分组，用清晰的标题
- 出了新问题就更新规则

**别这样做：**
- 写成小作文 -- CC 跟人一样，太长会跳着看
- 加入你从没用到过的规则 -- 每条规则都是认知负担
- 照搬通用建议 -- 规则应该反映你自己的工作流
- 忘了清理过时规则 -- 过时规则会稀释重要规则的注意力

### 规则结构

好规则都长这样：

```markdown
## [Category]

- [做什么/不做什么] ([原因/不这么做会怎样])
```

例子：
```markdown
## Safety

- Never use `rm` to delete files (use `mv` to `~/.Trash/` -- rm is unrecoverable)
- Change API/add params: grep ALL callers and confirm each (partial updates cause runtime errors)
```

### 反面教材

```markdown
# 太模糊 -- CC 无从下手
- Write good code

# 太长 -- CC 直接跳过
- When modifying any function that accepts parameters, you should always check
  every single file in the entire codebase to make sure that no other file
  references this function with the old parameter signature, because if you
  don't, you might end up with runtime errors that are hard to debug...

# 自相矛盾 -- CC 会懵
- Always ask before making changes
- Multi-step tasks: auto-confirm, don't stop
```

---

## CLAUDE.md 模板

完整带注释的模板在 [`templates/CLAUDE.md.template`](./templates/CLAUDE.md.template)，包含：

- 项目描述占位符
- 执行原则
- 诚实规则
- NEVER/ALWAYS 清单及说明
- 验证规则
- 回复纪律
- 编码规范占位符

每个部分都有 `<!-- CUSTOMIZE: ... -->` 注释，告诉你怎么根据自己的项目来改。

---

## 贡献

有什么规则帮你躲过一劫？欢迎提 PR！每条经验请按 [`lessons/README.md`](./lessons/README.md) 中的格式来写。

---

## 相关项目

- [**cc-hooks-gallery**](https://github.com/AliceLJY/cc-hooks-gallery) -- Claude Code 自动化 Hook 合集

---

## 许可证

MIT -- 有用的拿走，不合适的跳过。

---

> *"翻过车以后写下的规则，顶得上事前拍脑袋想的十条。"*
>
> -- 500+ 场 Claude Code 实战的经验之谈
