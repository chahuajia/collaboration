---
id: cli-agent-boundaries
type: integration
status: active
created: 2026-09-11
updated: 2026-10-06
applies-to:
  - cli-agent
author: heiniao
aliases:
  - cli-agent-boundaries
  - A7
provenance: 2026-09-16 由 A7-distribution-and-community 拆出：只保留"AI 写权限边界"这一半（可执行、今天在用）；社区治理那半面向 0 人社群，已归档（见 ADR-0007）。2026-10-06 修订：从"不 commit"改成"可 commit（带门禁 + 署名 + 任务分支）、不 push、不 merge 主干"
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

> **2026-10-06 修订**：原文是"**AI 不 commit、不 push**"。改了。
>
> **为什么改**：长任务里 agent 需要**切分支 / 合并 / 出错回滚** ——
> 而"改动只留在工作区"让 git 历史看不出过程、回滚只能靠人手工还原，协作者也读不到中间状态。
>
> **为什么原来那两条理由不构成反对**：本条目自己写的反对理由是
> "**commit 历史被噪音污染**" + "**没有身份机制：改动无法归因**" ——
> 两条都**可机制化**，不是"commit 本身危险"：
> 噪音 → **一任务一分支**；归因 → **`Generated-by:` 署名**。
>
> **顺带修掉一处"承诺 vs 现实"**：原来说"AI 不 push"，但 CLI 的 `collab push` **谁都能跑**
> （规则只写在散文里）。现在：CLI 侧 **默认拒绝**（要 `--allow-push` / `COLLAB_ALLOW_PUSH=1`），
> MCP 侧**根本没有 push 工具**。

| 动作 | 边界 |
| :--- | :--- |
| 读写工作区文件；`git status` / `log` / `diff` | 允许 |
| **建分支 / 切分支** | 允许，但**一任务一分支**（主干只接受人的 merge） |
| **commit** | 允许 —— 但**必须走 `collab commit`**（先 validate）+ **署名**（`Generated-by:`） |
| **`git push`** | ❌ **不**。需人显式授权：`collab push --allow-push`，或环境变量 `COLLAB_ALLOW_PUSH=1` |
| **merge 到主干** | ❌ 人的动作 |
| 持有远端凭据 | ❌ 不要在 AI 会话里传 token |

**一句话**：**AI 可以 commit（走 `collab commit`、带署名、在任务分支上），但不 push、不合并主干。**

**自证方式**（两条都可查）：

- 机器提交**可数**：`git log --format=%b | grep -c '^Generated-by:'`
- 主干上**没有** agent 的直接 push：`git log --format=%an main` 只应出现人

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
- **不要在没有分支纪律时 commit** —— 一任务一分支是"允许 commit"的**前提**；
  在主分支上连提交，就是原条目反对的那种"噪音污染"。
- 不要把 `--allow-push` / `COLLAB_ALLOW_PUSH` 当成默认值写进 agent 的环境 ——
  它是**人的**授权动作，不是配置项。
- 不要在 AI 会话里传入远端 token。
- 不要为"以后有社区"先写好治理流程 —— 那是"从未来设计"（0 人时无人执行）。

## 关联

[[A6-version-authority]] [[S10-collab-cli]] [[chatgpt-paste-protocol]]
