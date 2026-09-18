---
id: rfc-process
type: pattern
status: dormant
created: 2026-09-15
updated: 2026-09-17
author: heiniao
source: Rust RFC 流程
aliases: [rfc-process]
trigger: 约定级变更要不要走 RFC；或怕快速合并漏反对意见；或讨论散不开收不拢
provenance: 与 [[W7-rfc-process]] 重叠 ≥50% —— 2026-09-16 代谢：退出路由索引，保留文件作基因
enforced: null
---

# RFC 流程（已休眠）

> **dormant**：执行剧本以 [[W7-rfc-process]] 为准。本条与 W7 逐字级重复（提案 / 7 天讨论 / FCP / ADR），
> 按 [[meta/pruning-policy]] 判为**重复**，退出 `catalog` 路由；文件保留，需要时可复活。

## 上下文

约定级变更影响所有协作者，需要更审慎的流程。

## 问题

- 快速合并可能遗漏反对意见。
- 无限讨论则无法收敛。

## 方案

见 [[W7-rfc-process]]。

## 反面

- 不要同时维护本条与 W7 两套 RFC 步骤。

## 关联

[[W7-rfc-process]] [[patterns/peer-review]] [[rfcs/_index]]
