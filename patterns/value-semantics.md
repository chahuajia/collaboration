---
id: value-semantics
type: pattern
status: active
created: 2026-09-13
updated: 2026-09-17
applies-to:
  - all
supersedes:
author: heiniao
source: 工程实践（值语义）
aliases:
  - value-semantics
  - A11
trigger: 想跨域共享通用 0/空串常量；或同一个值在不同业务含义不清；或常量名读不出业务语义
provenance: 2026-09-16 从 agreements/ 降级为 pattern —— 它是通用的设计判据，不是"人和 AI 怎么协作"的规则
enforced: null
---

# 值同不代表语义同

> 原为 `A11-值同不代表语义同`（约定层）。2026-09-16 降级为 pattern：
> 它约束的是**代码怎么写**，不是**人和 AI 怎么协作** —— 按 `agreements/` 的新定义，它不该占宪法席位。
> 但它是通用判据（不绑语言、不绑栈），所以留在本库；**绑栈的那部分（TS/DDD 具体手法）归真实项目的规约**。

## 上下文

同一个值（如 `0`、空字符串、`null`）在不同业务上下文中，其含义完全不同。

## 问题

- 跨域共享"通用值"（如全局 `MathConstants.Zero`）会摧毁语义系统。
- 一旦有人改了共享常量，影响面不可控。
- 阅读时无法判断"这个 0 在业务上是什么"。

## 方案

- **值相同不代表语义相同**。当同一个值在不同上下文有不同业务含义时，必须在各自的领域**重新定义常量**。
- 禁止跨域共享"通用值"。
- 常量名必须能读出业务语义（如 `Severity.Error` 而非 `'error'`）。

## 反面

- 不要为了"避免重复"而把不同语义的同值常量合并。
- 不要用数字/字符串字面量表达业务含义。
- 不要用"通用的 0/1/-1"替代语义常量。

## 关联

[[patterns/design-decision]] [[S5-null-not-sentinel]] [[S17-ESLint-工具约束]]
