---
id: type-as-design
type: pattern
status: active
source: 类型论
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [type-as-design]
---

# 类型即设计

## 上下文

TS 的类型不只是注释，它是设计的一部分。

## 问题

类型退化成注释：`string` 表达一切、重复的类型运算到处复制，设计意图在类型层完全丢失。

## 方案

- 重复三次的类型推导 → 抽成工具类型。
- 有语义的 `string` → 用 branded type。
- 有约束的 id → 用 template literal type。
- 工厂的输入 → 用 `*Input` 命名，让工厂成为唯一消费者。
- DTO 不要伪装成领域对象。

## 反面

- 到处写 `typeof X[keyof typeof X]`。
- 用 `string` 表示一切。
- `*Input` / `*DTO` 混在 `domain/` 里。

## 关联

[[S15-TS-类型工厂]] [[S21-Branded-Type]] [[patterns/naming-as-definition]]
