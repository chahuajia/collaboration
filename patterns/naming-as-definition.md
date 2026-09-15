---
id: naming-as-definition
type: pattern
status: active
source: 语言哲学（命名即定义）
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [naming-as-definition]
---

# 命名即定义

## 上下文

需要命名类型、常量、函数、文件。

## 问题

命名与语言内建、主流框架或全局声明撞车，或者名字读不出它代表什么——歧义的名字会在半年后变成 bug。

## 方案

### 单一值 vs 集合

| 名字形态 | 命名模式 | 例子 |
| :--- | :--- | :--- |
| 像"单一值" | 同名模式（type + value） | `EntryId` |
| 像"集合/类别" | `*Values` + 同名 type | `EntryKindValues` + `EntryKind` |

### 避免环境冲突

命名前检查：

1. `@types/node` 里是否有全局声明？
2. 主流框架是否有同名？
3. DDD/CQRS 核心术语？（Entity、Port、Adapter、Command）
4. TS 内建？（Record、Array、Map、Set、Partial、Pick）

**任何一条命中 → 改名**。

### 语义精确

- 枚举值用 const object，不用裸字符串。
- 常量名必须能读出业务语义。

## 反面

- 不要用 `EntryType`（与 `perf_hooks.EntryType` 冲突）。
- 不要用 `Record`（与 TS 内建冲突）。
- 不要用"通用的 0"替代语义常量。

## 关联

[[S15-TS-类型工厂]] [[A11-值同不代表语义同]] [[patterns/allowlist-over-denylist]]
