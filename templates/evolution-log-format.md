---
id: evolution-log-format
type: meta
status: active
created: 2026-09-11
updated: 2026-09-11
---

# evolution-log 格式规范

## 上下文

演化日志记录 COLLABORATION 的所有正式变更。需要明确的字段定义，避免记录退化。

## 方案

### 字段

| 字段 | 含义 | 必填 | 示例 |
| :--- | :--- | :--- | :--- |
| 日期 | 变更生效日 | 是 | 2026-09-11 |
| 版本 | 语义化版本 | 是 | v4.0.2 |
| 变更 | 一句话描述 | 是 | 新增约定 A9 |
| 提议者 | 谁提出 | 是 | user@example.com 或 AI |
| 确认者 | 谁批准生效 | 是 | 约定级=双方；技能级=单人 |
| commit | 关联 git commit | 否 | 前 7 位；未提交用 `-` |
| 关联 | 涉及条目 ID | 否 | A8, A9 |

### 确认者规则（承接 A6）

| 层级 | 确认者 |
| :--- | :--- |
| 约定 | 双方 |
| 工作流 | 用户 |
| 技能 | 单人 |
| 模式 | 单人 |
| ADR | 双方 |

### 身份标识

- 用户：git email。
- AI：统一写 `AI`。
- 多人：逗号分隔。
- 未确定：`user@local (待补)`。

## 反面

- 不修改历史条目。
- 不用假 email。
- 不省略确认者。
- 不把 commit 当成必填（未提交时用 `-`）。

## 关联

[[A4-proactive-update]] [[A6-version-authority]] [[meta/evolution-log]]