---
id: S14
type: skill
status: active
created: 2026-09-13
updated: 2026-09-17
domains:
  - ddd
applies-to:
  - W3
supersedes:
author: heiniao
aliases:
  - S14
trigger: Issue 用 DTO 丢行为；或散落 severity 字面量
enforced: null
---

# S14 Issue 值对象

## 上下文

需要表达"校验发现的问题"。

## 问题

- 用 DTO 承载 Issue 会丢失行为。
- 散落字面量（`severity: 'error'`）无法集中管理。

## 方案

- Issue 是**有行为的值对象**（不是 DTO）：
  - `isBlocking()`
  - `format()`
  - `withSuggestion()`
  - `equals()`
- **工厂集中在 `Issues` 命名空间**（或 `Issue` 静态方法）。
- 业务代码中不出现裸字面量。

## 反面

- 不要把 Issue 做成实体（Issue 无独立身份）。
- 不要在规则里手写 Issue 结构。

## 关联

[[S13-Smart-Constructor]] [[S17-ESLint-工具约束]]
