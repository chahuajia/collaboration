---
id: S36
type: skill
status: active
created: 2026-09-17
updated: 2026-09-17
author: heiniao
aliases:
  - S36
  - agent-workspace-boundaries
domains:
  - meta
  - tooling
applies-to:
  - multi-agent
  - monorepo
provenance: evolutionary phase-7 FE∥BE 集群——子 agent 与父会话共用工作树，无目录沙箱；投递凭据见 parallel-work-needs-delivery-proof
---

# S36 Agent 工作区边界（FE/BE 集群）

## 上下文

一人 + 多 agent 在 **monorepo** 上并行。没有独立 `frontend.git` / `backend.git` 时，
「工作空间」不是再拆远程，而是 **路径权限 + WM 沙箱 + 分支**。

## 问题

- 集群已派出，但 `frontend/` / `backend/` 下没有 agent 进度落点 → 状态只在父会话上下文。
- 子 agent 越权改对方目录或与父会话抢同一文件，commit 揉并。
- 误以为「要先拆仓」才能并行。

## 方案

### 三级（由轻到重）

| 级 | 做法 | 何时 |
| :--- | :--- | :--- |
| **L0 约定** | `loop.md` 写明：FE 只碰 `frontend/`；BE 只碰 `backend/`（+ 约定包路径） | 每个并行 phase **立即** |
| **L1 WM 沙箱** | `working-memory/agents/fe/` · `be/` 放该 agent 的 `loop.md` / 笔记（**不**放业务源码） | 下一次并行起强制 |
| **L2 git** | `wip/pN-fe-*` / `wip/pN-be-*`；父会话 merge | 已有分支约定时强制 |
| **L3 worktree** | 每 agent 一棵工作树 | 冲突频繁再上 |

### 派出时必落盘（与投递凭据对齐）

```text
working-memory/agents/<role>/
  loop.md       ← 范围、禁区、验收、停条件
  status.md     ← 进行中 / 阻塞 / 完成
```

任务级 brief 仍可放在 `working-memory/tasks/<task>/`（见 [[patterns/parallel-work-needs-delivery-proof]]）。

### Monorepo 裁决

拆 FE/BE **仓**的门槛：独立发布 / 权限 / 团队。未达门槛 → 用本技能，不拆远程。

## 反面

- 不要在 `frontend/` 里建第二份 `working-memory/` 当知识库。
- 不要让 FE agent 改 `backend/src/main/java`（除非 brief 显式授权且单文件契约）。
- 不要用「再派一个 agent」代替写清路径边界。

## 关联

[[patterns/parallel-work-needs-delivery-proof]] [[patterns/pressure-routing]] [[W10-working-memory]] [[cli-agent-boundaries]]
