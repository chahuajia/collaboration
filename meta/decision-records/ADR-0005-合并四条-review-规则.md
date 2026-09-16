---
id: ADR-0005
type: adr
status: accepted
date: 2026-09-16
author: heiniao
created: 2026-09-16
updated: 2026-09-16
aliases: [ADR-0005]
provenance: 五份外部评估独立指出 A10 / W8 / S27 / review-marginal-value 四条讲同一件事（320 行）
---

# ADR-0005 合并四条 review 规则为 A10

## 背景

`agreements/A10-review-前置原则`、`workflows/W8-规格优先的-AI-协作流程`、
`skills/S27-规格优先-review`、`patterns/review-marginal-value` 四条共 320 行，讲的是同一件事。

重复是逐字级的：

- "review 对象优先级：规格 > 测试 > 类型 > 实现" —— 四条里各出现一次。
- "五阶段流程" —— A10 与 W8 逐字重复。
- "五个对策"表格 —— A10 / W8 / review-marginal-value 里出现三次。
- "用户 review 的是意图，不是代码" —— 四条里各出现一次。
- `review-marginal-value` 自己写着"本 pattern 是 A10 的'为什么'" —— 等于承认它是一次再展开，不是新知识。

而 `meta/pruning-policy` 明文规定"两条目重叠 ≥ 50% → 合并"，这四条的重叠远超 50%。
**规则写了，但从未执行**：`evolution-log` 的 107 次记录里，0 次删除、0 次合并。

## 决策

四条**合并为一条**：`agreements/A10-review-前置原则.md`（id 保持 `A10`）。

- 删除 `workflows/W8-规格优先的-AI-协作流程.md`
- 删除 `skills/S27-规格优先-review.md`
- 删除 `patterns/review-marginal-value.md`
- 旧编号 `W8` / `S27` / `review-marginal-value` 保留在 A10 的 `aliases`（便于按旧名检索）
- 全库引用改指 A10，模板与索引同步

**为什么留在 `agreements/` 而不是 workflow**：这四条讲的是"人和 AI 怎么分工 review"，
是**跨栈不变的行为约束**，不是某个技术栈的做法 —— 按"协议 vs 手册"的分界，它属于协议。

**顺带删除的内容**：原文里"边际价值增加 3-5 倍""等价于读 1000 行代码"这类数字没有来源、
不可验证，正是 [[patterns/design-decision]] 禁止的"拿最佳实践当理由"。合并时删除。

## 后果

正面：

- 320 行 → 约 130 行；一个概念只有一处定义。
- 这是本库**第一次真正的"选择"事件** —— 此前只有变异，没有选择。
- 验证：`collab validate` 105 entries / 0 issues。

负面：

- 旧编号变成历史名词，检索要靠 `aliases`。
- 合并动到 14 个文件的引用（含模板与索引）—— 一次高风险的批量改动。
  兜底是 `collab validate` 的死链检查：**它现在是 0，这就是本次改动的验收标准**。

## 替代方案

- **保留四条、只加交叉引用** —— 否决：重叠仍在，读者仍要读四遍。
- **并入 W8 而不是 A10** —— 否决：A10 是被引用最多的条目（20 处），改名成本高；
  且"review 对象优先级"本身是约定，不是流程。
- **拆成两条（约定留规则、工作流留流程）** —— 暂缓：那属于"协议 / 手册"大拆分，
  本次只做"4 → 1"这一个动作。

## 关联

[[A10-review-前置原则]] [[meta/pruning-policy]] [[W9-Spike-工作流]]
