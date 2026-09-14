---
id: rfc-process
type: pattern
status: active
source: Rust RFC 流程
---

# RFC 流程

## 上下文

约定级变更影响所有协作者，需要更审慎的流程。

## 问题

- 快速合并可能遗漏反对意见。
- 无限讨论则无法收敛。

## 方案

- **提案**：写入 `rfcs/`。
- **讨论期**：≥ 7 天。
- **FCP**：3 天，无异议则合并。
- **有异议**：回讨论期或核心团队裁决。
- **合并**：写 ADR。

## 反面

- 不要用于技能级变更。
- 不要在 FCP 期间强推。

## 关联

[[W7]] [[patterns/peer-review]] [[rfcs/_index]]