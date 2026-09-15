---
id: ADR-0003
type: adr
status: accepted
date: 2026-09-13
author: <待填>
created: 2026-09-15
updated: 2026-09-15
aliases: [ADR-0003]
---
# ADR-0003 采用 Spike 前置阶段
## 背景
TDD 假设"问题空间已明确"。当问题空间不明确（如 `collab new` 的模板该长什么样）时，TDD 会失真——写出"我以为是这样的"测试。
## 决策
在 A10 流程的"规格"阶段之前，插入可选的 Spike 阶段。
- Spike 目标：学习。
- Timebox：默认 2 小时。
- 代码：不保留。
- 产出：写入规格的知识。
## 后果
正面：
- 避免错误 TDD 的成本。
- 用最少成本回答"能不能做"和"接口该长什么样"。
- 保持 A10 的严谨性——Spike 结束后进入正式流程。
负面：
- 增加一轮迭代时间。
- 可能被滥用为"跳过 TDD"的借口。
## 替代方案
- 直接 TDD：否决，问题空间不明确时无法写出正确测试。
- 完全跳过 Spike：同上。
## 关联
[[A10-review-前置原则]] [[W8-规格优先的-AI-协作流程]] [[W9-Spike-工作流]] [[patterns/cross-domain-borrowing]]
