---
id: load-bearing-vs-partition
type: pattern
status: active
source: 建筑
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [load-bearing-vs-partition]
trigger: 不确定改的是约定还是技能；或把技能当承重墙改不动；或约定被当隔断随便改
---

# 承重墙 vs 隔断

## 上下文

不同内容修改成本和影响范围不同。

## 问题

如果所有内容都可以随意改，协作基线不稳定。

## 方案

- **承重墙**（约定）：双方确认才能改。
- **隔断**（技能）：自由增删、合并、废弃。
- **中间层**（工作流）：AI 提议，用户确认。
- 结构性决策写 ADR。

## 反面

- 不要把技能当承重墙，迭代太慢。
- 不要把约定当隔断，基线不稳。

## 关联

[[A6-version-authority]] [[W3-ddd-refactor]] [[S7-ddd-frontend-light]] [[ROOT]]