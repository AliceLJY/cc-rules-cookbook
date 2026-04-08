# cc-rules-cookbook

**跨学科的 Claude Code 驾驭框架 -- 认知心理学、风险评估、ReAct 方法论与工程纪律。源自 500+ 场实战。**

<p align="center"><a href="README.md">English</a></p>

[![Frameworks](https://img.shields.io/badge/frameworks-5-blue)](./frameworks/)
[![Methodology](https://img.shields.io/badge/methodology-6%20systems-orange)](./methodology/)
[![Architecture](https://img.shields.io/badge/architecture-3%20patterns-green)](./architecture/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## 这是什么

这不是一份"别做 X"的规则清单。它是一套跨学科体系，将认知心理学、风险评估、AI 研究和工程方法论应用到 Claude Code 工作流中。

基础规则（"别用 rm"、"关窗前 commit"）放在 [quick-reference/](./quick-reference/) 里。它们很重要，但不是亮点。亮点是框架 -- 系统性地解决人机协作中的难题：如何从一个"想讨好你"的系统中获得诚实输出，如何在 code review 中过滤信噪比，如何用机械约束替代意志力来执行纪律。

这里的所有内容都从 500+ 场实战中提炼，没有一条是纸上谈兵。

---

## 框架

### [偏差校正矩阵](./frameworks/bias-correction-matrix.md)

七种系统性扭曲 AI 代码审查的认知偏差，及其机械化对策。

| 偏差 | 表现 | 对策 |
|------|------|------|
| 讨好偏差 | 为维持正面互动而夸赞代码 | "表扬不是你的工作。你的工作是找问题" |
| 长度偏差 | 把冗长代码当作更严谨 | "惩罚冗长。Concise > lengthy" |
| 权威偏差 | 把自信的代码当作正确的 | "置信度不等于正确性" |
| 完成偏差 | 只要"写完了"就给分 | "垃圾也可以是完整的" |
| 努力偏差 | 看出投入大就手下留情 | "努力不相关。只评判 OUTPUT" |
| 新近偏差 | 把新模式当作更好的 | "新不等于好" |
| 熟悉偏差 | 把常见模式当作正确的 | "常见不等于正确" |

默认评分：**2/5**。必须用证据把分数**往上**争。5/5 出现频率低于 5%。

另有六条反合理化规则，封堵"基本还行"（= 部分有问题 = FAIL）和"人家很努力"（努力不相关）这类逃逸路径。

### [置信度 x 影响度过滤器](./frameworks/confidence-impact-filter.md)

代码审查发现项的量化二维过滤系统，借鉴风险评估方法论。

```
置信度
100 |  报告    报告    报告    报告    报告
 95 |  报告    报告    报告    报告    报告
 85 |  ----    报告    报告    报告    报告
 75 |  ----    ----    报告    报告    报告
 65 |  ----    ----    ----    报告    报告
 50 |  ----    ----    ----    ----    报告
  0 |  ----    ----    ----    ----    ----
     琐碎     低      中等     高      关键
                     影响度 -->
```

关键级发现（安全、数据丢失）在 50% 置信度就汇报。琐碎发现（代码风格、命名）需要 95% 置信度。阈值以下的发现**直接丢弃** -- 不写"顺便一提"，不写"小建议"。

### [反讨好协议](./frameworks/anti-sycophancy.md)

行为心理学在 AI 提示中的应用。核心洞察：AI 告诉你想听的话，因为训练奖励了这种行为。

| 有偏提示 | 中性提示 |
|---------|---------|
| "找出这段代码的 bug" | "通读逻辑，报告所有发现" |
| "这段代码有性能问题" | "分析这段代码的性能特征" |
| "这个方案是不是更好？" | "从 X、Y、Z 维度对比两个方案" |

高风险决策：三 agent 对抗验证。A 生产、B 攻击、C 仲裁。

### [XY 问题检测](./frameworks/xy-problem-detection.md)

前置的手段-目标错配识别。用户问的是 Y（解决方案）而不是 X（问题本身）时，先指出再执行。

### [完成状态分类法](./frameworks/completion-taxonomy.md)

四种状态替代模糊的"完成了"：

| 状态 | 含义 |
|------|------|
| **DONE** | 测试通过、lint 干净、无遗留 TODO |
| **DONE_WITH_CONCERNS** | 完成但有风险 -- 逐条列出 |
| **BLOCKED** | 无法继续 -- 说明需要什么 |
| **NEEDS_CONTEXT** | 信息不足，无法有效开始 |

---

## 方法论

### [Research -> Plan -> Implement](./methodology/research-plan-implement.md)

复杂任务的三阶段纪律。不是建议 -- 是要求。

```
Research          Plan                Implement
                  (标注循环)           (ReAct 循环)
                  
research.md  -->  plan.md         --> 代码变更
人类审阅          人类标注              逐步执行
                  [注：...]           Act/Observe/
                  CC 更新              Reflect/Record
                  循环 1-N 轮
                  不改代码
```

核心创新：`plan.md` 是**共享可变状态**。人类直接在文件中加 `[注：...]` 标注。AI 处理标注并更新计划。循环到人类满意为止。计划定稿前不动代码。

### [ReAct 循环](./methodology/react-loop.md)

步骤级别的观察，源自 AI 研究（Yao et al., 2022）。每个动作之后：读实际结果、对比预期、记录发现。意外结果 -> 停下来。方向错了 -> 回退，不打补丁。

可通过 hook 执行：5 次以上工具调用没有观察记录就触发自动提醒。

### [范围漂移检测](./methodology/scope-drift-detection.md)

实施后审计。对比 `plan.md` 和 `git diff --stat`：
1. 改了计划之外的文件？= 范围蔓延
2. 计划中的步骤没有对应变更？= 需求遗漏

输出：`Scope Check: CLEAN / DRIFT DETECTED / REQUIREMENTS MISSING`

### [任务契约](./methodology/task-contracts.md)

`{TASK}_CONTRACT.md` 在开工前定义完成标准。"Agent 没满足契约 = 没完成。"不同契约用不同会话。方向错了 -> 回退重来。

### [驾驭工程](./methodology/harness-engineering.md)

这是本体系与"技巧清单"的根本区别：**纸面规则是建议，代码规则是约束。**

```
规则被反复违反 --> 写一个 hook --> 规则变成机械执行
```

例子：
- "别用 rm" -> hook 拦截 rm 并返回 exit 2
- "关窗前 commit" -> stop hook 检测脏仓库
- "提 PR 前查重" -> hook 拦截 `gh pr create` 并提醒
- "写观察笔记" -> hook 检测 5+ 次工具调用无记录

同一规则被违反三次 -> 从 CLAUDE.md 升级为 hook。

### [技能进化](./methodology/skill-evolution.md)

系统化提炼：重复工作流 -> 技能，体验差的技能 -> 更新，一次性脚本 -> 工具箱，行为模式 -> 规则。

---

## 架构

### [记忆架构](./architecture/memory-architecture.md)

三维记忆（时效性 x 相关性 x 重要性）+ 双写策略（本地文件 + 外部服务）。压缩优先级：架构决策 > 修改文件 > 验证状态 > 待办事项 > 工具输出。

### [回复纪律](./architecture/reply-discipline.md)

三条规则：回复前扫描未处理项、完整阅读工具输出、侧线任务结束后回到主线。

### [置信度标注](./architecture/confidence-tagging.md)

诚实分类法：`[已验证]` / `[待确认]` / `[低置信]`。区分"我知道"和"我在猜"。

---

## 快速上手

不要从安全清单开始。从你最大的痛点对应的框架开始：

| 你的问题 | 从这里开始 |
|---------|-----------|
| Code review 没用 -- 太客气、太多废话 | [偏差校正矩阵](./frameworks/bias-correction-matrix.md) + [置信度 x 影响度过滤器](./frameworks/confidence-impact-filter.md) |
| AI 什么都同意 | [反讨好协议](./frameworks/anti-sycophancy.md) |
| 复杂任务产出混乱 | [Research -> Plan -> Implement](./methodology/research-plan-implement.md) |
| 规则总被忘记 | [驾驭工程](./methodology/harness-engineering.md) |
| "完成了"不等于真完成 | [任务契约](./methodology/task-contracts.md) + [完成状态分类法](./frameworks/completion-taxonomy.md) |
| 需要基本操作安全规则 | [安全检查清单](./quick-reference/safety-checklist.md) |

开箱即用模板：[`templates/CLAUDE.md.template`](./templates/CLAUDE.md.template) 和 [`templates/workflow.md.template`](./templates/workflow.md.template)。

---

## 适合谁

用 Claude Code 用到一定程度的人 -- 意识到原始算力不够，你需要转向系统。你撞过那堵墙：AI 有能力但不可靠，指令有时遵守有时忘，"完成"含义模糊，review 全是好话。

这本手册就是转向系统。不是更多规则 -- 是更好的框架。

---

## 相关项目

- [**cc-hooks-gallery**](https://github.com/AliceLJY/cc-hooks-gallery) -- Claude Code 自动化 Hook 合集（驾驭工程的执行层）

---

## 许可证

MIT -- 有用的拿走，不合适的跳过。
