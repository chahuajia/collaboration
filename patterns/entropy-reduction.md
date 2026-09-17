---
id: entropy-reduction
type: pattern
status: active
source: 物理学（熵增定律）
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [entropy-reduction]
trigger: 知识库只增不减开始烂；或不舍得删曾经有用的；或找一条要翻很久
---

# 熵减机制

## 上下文

不主动维护，知识库必然腐烂。

## 问题

条目越来越多，无人清理，最终没人读。

## 方案

主动输入负熵：
- 定期修剪（见 [[meta/pruning-policy]]）。
- 合并重叠条目。
- 标记 dormant，再删除。
- 保持 30 秒查找成本。

## 反面

- 不要只增不减。
- 不要害怕删除“曾经有用”的内容。

## 关联

[[patterns/evolution-loop]] [[meta/pruning-policy]] [[ROOT]]