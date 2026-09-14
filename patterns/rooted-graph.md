---
id: rooted-graph
type: pattern
status: active
created: 2026-09-11
updated: 2026-09-14
source: 图论
author: heiniao
---

# 有根图

## 上下文

知识结构需要同时支持层级查找和横向关联。

## 问题

纯树结构会导致：
- 深路径找不到。
- 同一概念在多分支重复。
- 无法横切检索。

## 方案

- 主结构是树（目录层级）。
- 每个条目带 YAML 元数据（横切标签）。
- 条目之间用 `[[ID]]` 双向链接。
- 结果：有根图（Rooted Graph），树 + 横切 + 双向。

## 反面

- 不要放弃树结构，纯图会失去层级感。
- 不要过度链接，只链真正相关的。

## 关联

[[S9]] [[patterns/faceted-classification]] [[ROOT]]