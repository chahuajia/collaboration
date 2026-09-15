---
id: rooted-graph
type: pattern
status: active
source: 图论
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [rooted-graph]
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
- 条目之间用**双链语法**（双方括号包裹条目 ID）互相链接。
- 结果：有根图（Rooted Graph），树 + 横切 + 双向。

## 反面

- 不要放弃树结构，纯图会失去层级感。
- 不要过度链接，只链真正相关的。

## 关联

[[S9-yaml-metadata]] [[patterns/faceted-classification]] [[ROOT]]
