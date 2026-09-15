---
id: what-how-are-projections
type: pattern
status: active
source: 投影几何 + 相对论
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [what-how-are-projections]
---

# What / How 是同一事物的两个投影

## 上下文

设计时反复遇到"这是 What 还是 How"的困惑。

## 问题

- **直觉**：世界里有"两类东西" —— 一些是 What（意图）、一些是 How（实现）。
- **真相**：没有绝对分类 —— **每个东西同时是 What 和 How** —— 取决于观察视角。

## 方案

### 核心：What / How 是相对的

- **对 A 是 What** —— 对 B 可能是 How。
- **对内层是 What** —— 对外层可能是 How。

### 四层动态

| 层 | 含义 |
| :--- | :--- |
| **传导** | 每层对下是 What、对上是 How |
| **演化** | 今天的 How 是明天的 What（第二处使用时） |
| **传染** | 一处错位会横向扩散（复制）和纵向传导（漂移） |
| **相对** | 视角切换时，边界也切换 |

### 判据

**判断一个东西是 What 还是 How**：

1. **它被几处使用？** —— 1 处 = How，2+ 处 = What 候选。
2. **相对谁？** —— 相对于它服务的对象。
3. **它稳定还是易变？** —— 稳定 = 更接近 What。

### 应用

| 场景 | What | How |
| :--- | :--- | :--- |
| **值对象 vs 应用服务** | 值对象 | 应用服务（编排） |
| **领域层 vs 适配器** | 领域层 | 适配器 |
| **测试的 Act/Assert vs Arrange** | Act / Assert | Arrange |

### 提炼"独立的 What"的判据

**从 How 升为 What 的触发条件**：

- 被 2+ 处使用。
- 语义稳定（不随调用方变化）。
- 有独立命名价值。

## 反面

- 不要假设"What 和 How 是两类东西"。
- 不要用"重复几次"作为唯一判据 —— 还要看语义。
- 不要为"未来可能复用"提前提取 —— YAGNI。

## 关联

[[S12-边界解析]] [[patterns/domain-purity-is-structural]]
