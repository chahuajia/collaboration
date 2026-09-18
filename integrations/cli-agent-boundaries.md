---
id: cli-agent-boundaries
type: integration
status: active
created: 2026-09-11
updated: 2026-09-17
applies-to:
  - cli-agent
author: heiniao
aliases:
  - cli-agent-boundaries
  - A7
provenance: 2026-09-16 由 A7-distribution-and-community 拆出：只保留"AI 写权限边界"这一半（可执行、今天在用）；社区治理那半面向 0 人社群，已归档（见 ADR-0007）
trigger: AI 有写权限不知边界；或不该 commit/push 却做了
enforced: null
---

# cli-agent：写权限与身份边界

> 原 `A7-distribution-and-community` 装着两半：**① AI 的写权限边界**（本条目）、
> **② 社区治理**（分发分层、RFC 门槛、许可证）。
>
> **关键修正**：原文写"AI 永远不持有主干写权限" —— **这不是事实**。
> 有文件权限的 agent 确实持有写权限。**一条无法执行的规则会稀释全部规则的权威**，
> 所以改成可执行、且能自查的版本。

## 上下文

AI 在这个环境里**有**文件读写与 git 权限。所以问题不是"要不要给权限"，
是"**有权限时怎么用**"。

## 问题

- 规则说"AI 没有写权限"，事实相反 → 规则被无视，且每轮都在违反它。
- AI 直接 `commit` / `push`：commit 历史被噪音污染，上下文注入的代价被放大。
- 没有身份机制：改动无法归因，演化无法追溯。

## 方案

### 一、行为边界（可执行）

| 允许 | 禁止 |
| :--- | :--- |
| 读写工作区文件 | **`git commit`** |
| `git status` / `log` / `diff`（只读查询） | **`git push`** |
| 人在场时按指令 `git add` | 持有远端凭据 |

**一句话**：**AI 不 commit、不 push；改动只留在工作区，等人 review。**

**自证方式**：`git log` 里没有 AI 的提交；工作区始终是"已改未提交"。

### 二、身份三层

| 层 | 来源 | 作用 |
| :--- | :--- | :--- |
| Git 身份 | `git config user.name` / `user.email` | 真相来源，可验证 |
| YAML 元数据 | 条目头的 `author` / `provenance` | 归因与溯源 |
| 未确定 | `AI`（AI 产出） | — |

### 三、归因

- `author` 默认取 **`git config user.name`**（`collab new` 已实现）。
- `provenance` 必须保留来源，**禁止洗稿**。

## 反面

- 不要把"AI 不 commit"读成"AI 没有权限" —— 前者是行为约束，后者是错误陈述。
- 不要因为"人忘了 review"就让 AI 顺手 commit。
- 不要在 AI 会话里传入远端 token。
- 不要为"以后有社区"先写好治理流程 —— 那是"从未来设计"（0 人时无人执行）。

## 关联

[[A6-version-authority]] [[S10-collab-cli]] [[chatgpt-paste-protocol]]
