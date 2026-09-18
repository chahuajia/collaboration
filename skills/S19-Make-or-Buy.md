---
id: S19
type: skill
status: active
created: 2026-09-13
updated: 2026-09-17
domains:
  - meta
applies-to:
  - all
supersedes:
author: heiniao
aliases:
  - S19
trigger: 新问题不知引依赖还是自研；或怕盲目造轮子/堆依赖
enforced: null
---

# S19 Make-or-Buy（货架商品）

## 上下文

面对一个新问题，是否引入现成依赖？

## 问题

- 盲目引入依赖导致复杂度膨胀。
- 盲目自研导致重复造轮子。

## 方案

### 三步决策

| 步 | 动作 |
| :--- | :--- |
| 1. 侦查 | 搜索 GitHub / npm / 论坛 |
| 2. 分析 | 评估成熟度、维护状态、依赖成本 |
| 3. 决策 | 引入 / 自研 / 简化后自研 |

### 判据

- **自己实现 < 50 行且无边界情况** → 自研
- **现成方案成熟且成本可控** → 引入
- **现成方案太重但思路有用** → 简化后自研

## 反面

- 不要因为"以后可能会用"而引入。
- 不要因为"自己写能学东西"而重复造轮子。
- 不要把依赖当成"免思考的借口"。

## 关联

[[patterns/design-decision]] [[patterns/dependency-decision]]
