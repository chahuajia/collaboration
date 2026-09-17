---
id: faceted-classification
type: pattern
status: active
source: 图书馆学
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [faceted-classification]
trigger: 只能按路径找条目；或想按领域/状态横切却没标签；或标签只标了一个维度
---

# 分面分类

## 上下文

条目增多后，单一维度检索不够。

## 问题

只能按路径找，无法按领域/状态/适用阶段横切。

## 方案

每个条目带多维度标签：
- `type`: agreement / workflow / skill / pattern / domain
- `status`: draft / active / dormant / deprecated
- `domains`: 跨领域
- `applies-to`: 被哪些工作流调用

## 反面

- 不要标签泛滥。
- 不要只标一个维度。

## 关联

[[S9-yaml-metadata]] [[patterns/rooted-graph]] [[ROOT]]