---
id: peer-review
type: pattern
status: active
source: 学术同行评审
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [peer-review]
trigger: 约定级变更要不要别人复核；或差点自审自合；或复核变成橡皮图章/拖慢简单改动
---

# 同行评审

## 上下文

单人提交容易有盲区，尤其是约定级变更。

## 问题

- 作者视角有局限。
- 无复核则质量不可控。

## 方案

- 每个约定/工作流变更，至少 1 名非作者复核。
- 技能级变更可选复核，但 CI 必须通过。
- 复核者关注：YAML 完整性、反面章节、双向链接、是否与现有条目冲突。
- 使用 GitHub Review 功能，禁止自审自合。

## 反面

- 不要让复核变成橡皮图章。
- 不要用复核拖延简单变更。

## 关联

[[cli-agent-boundaries]] [[W6-local-patch-to-community-pr]] [[patterns/rfc-process]]