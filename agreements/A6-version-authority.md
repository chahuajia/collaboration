---
id: A6
type: agreement
status: active
created: 2026-09-11
updated: 2026-09-11
applies-to: [all]
supersedes: null
author: heiniao
aliases: [A6]
---

# A6 版本与修改权限

## 上下文

不同层级的内容，修改成本和影响范围不同，权限应分层。

## 问题

- 如果约定可单方面改，协作基线就不稳定。
- 如果技能也要双方确认，迭代效率太低。

## 方案

| 层级 | 修改权限 |
| :--- | :--- |
| 约定 | 双方明确确认 |
| 工作流 | AI 可提议，用户确认 |
| 技能 | 自由增删、合并、废弃 |
| 结构性决策 | 写 ADR 到 `meta/decision-records/` |

版本号遵循语义化：v主版本.次版本.修订。

## 反面

- 不要在未确认时改约定。
- 不要为技能修改写 ADR，太重。
- 不要跳过 evolution-log。

## 关联

[[A4-proactive-update]] [[meta/evolution-log]] [[ROOT]]（ADR 目录见 `meta/decision-records/`）
