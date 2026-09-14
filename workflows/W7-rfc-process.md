---
id: W7
type: workflow
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [meta]
applies-to: [agreement-change, structural-change]
supersedes: null
author: heiniao
aliases: [W7]
---

# W7 RFC 流程（约定级变更）

## 上下文

约定和结构性决策影响所有协作者，需要更高门槛。

## 问题

- 若约定可随意改，协作基线不稳。
- 若没有讨论期，容易遗漏反对意见。

## 方案

借鉴 Rust RFC 流程：

1. **提案**：在 `rfcs/` 下新建 `RFC-XXXX-<name>.md`，用 `templates/rfc-template.md`。
2. **讨论期**：至少 7 天，任何协作者可评论。
3. **Final Comment Period（FCP）**：3 天，标记 `fcp` 标签。
4. **无异议**：进入 PR 流程。
5. **有异议**：回到讨论期，或由核心团队裁决。
6. **合并**：更新 `agreements/` 或 `meta/decision-records/`，写 ADR。
7. **记录**：更新 `evolution-log.md`。

RFC 状态：`draft → discussion → fcp → accepted / rejected / withdrawn`。

## 反面

- 不要用 RFC 流程处理技能级变更，太重。
- 不要在 FCP 期间强行合并。
- 不要跳过 ADR。

## 关联

[[A6-version-authority]] [[A7-distribution-and-community]] [[W6-local-patch-to-community-pr]] [[patterns/rfc-process]] [[rfcs/_index]]