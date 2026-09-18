---
id: value-object-as-raw-material
type: pattern
status: active
source: 制造业（原料-商品-工厂）
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [value-object-as-raw-material]
trigger: 值对象被当成 DTO/数据容器；或不变量不知道归谁；或差点给无行为 DTO 写工厂
enforced: null
---

# 值对象是原料

## 上下文

DDD 战术设计中，值对象的定位不清晰。

## 问题

值对象被当成 DTO / 数据容器，于是**不变量没有归属**，工厂与实体失去了地基。

## 方案

| DDD 概念 | 比喻 | 说明 |
| :--- | :--- | :--- |
| **值对象** | **原料** | 最基本的、不可变的、有业务含义的单位 |
| 实体 | 商品 | 有身份、有生命周期 |
| 工厂 | 生产线 | 保证原料/商品满足不变量 |
| 仓储 | 仓库 | 存储与取出 |
| 领域服务 | 跨工厂流程 | 需要多个工厂协作 |

**没有原料，商品造不出来。原料不合格，商品全废。**

## 反面

- 不要把 DTO 伪装成值对象。
- 不要为无业务行为的 DTO 写工厂。
- 不要让值对象成为"数据容器"。

## 关联

[[S13-Smart-Constructor]] [[S14-Issue-值对象]] [[patterns/type-as-design]]
