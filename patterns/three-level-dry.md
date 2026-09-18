---
id: three-level-dry
type: pattern
status: active
source: 软件工程（DRY）
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [three-level-dry]
trigger: 把看起来相同的代码强行合并；或已有 Result 又写 Either；或新问题不先查现成方案就从零造
enforced: null
---

# 不重复原则的三个层次

## 上下文

DRY 是常识，但常被误解为"代码文本不重复"。

## 问题

DRY 被缩成"代码文本不重复"：不同语义的相同文本被强行合并（错误耦合），而同一知识在代码、系统、知识三个层次上被反复重造。

## 方案

### 层次一：代码级 DRY

- **DRY 针对"知识"，不针对"代码文本"**。
- 两个看起来相同的代码，如果业务含义不同，强行合并会导致耦合。

### 层次二：系统级复用

- 不要重复封装（已有 `Result`，不再写 `Either`）。
- 组合优于重复。
- 扩展优于修改。

### 层次三：知识级复用（货架商品）

- 遇到新问题，先侦查现成方案。
- 分析是否值得引入。
- 从零实现 vs 引入依赖 vs 简化后自研。

## 反面

- 不要因为"文本相同"就合并。
- 不要因为"想学"就重复造轮子。
- 不要因为"想省事"就盲目引入。

## 关联

[[S19-Make-or-Buy]] [[patterns/dependency-decision]]
