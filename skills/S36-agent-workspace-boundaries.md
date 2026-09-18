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
provenance: evolutionary phase-7 FE∥BE；极端无人托管；v4 吞吐：加厚 brief / 分仓契约 / 目标测
trigger: FE∥BE 多 agent 工作区怎么划；或不想为此拆仓却怕抢文件；或前后端仓权限分离怎么协同；或集群切片太碎推进慢
enforced: null
---

# S36 Agent 工作区边界（FE/BE 集群）

## 上下文

一人 + 多 agent 在 **monorepo** 上并行。没有独立 `frontend.git` / `backend.git` 时，
「工作空间」不是再拆远程，而是 **路径权限 + WM 沙箱 + 分支**。

若权限已分仓，协同面换成 **契约仓/包 + 中立编排 WM**，不是取消本技能。

极端无人托管时，本技能是**下限**；完整门禁见 [[patterns/extreme-unattended-cluster]]。

## 问题

- 集群已派出，但 `frontend/` / `backend/` 下没有 agent 进度落点 → 状态只在父会话上下文。
- 子 agent 越权改对方目录或与父会话抢同一文件，commit 揉并。
- 误以为「要先拆仓」才能并行。
- 分仓后两边测绿、**契约口对不上**。
- 切片过碎（半截岛）导致集群吞吐塌方。

## 方案

### 三级（由轻到重）— monorepo

| 级 | 做法 | 何时 |
| :--- | :--- | :--- |
| **L0 约定** | `loop.md` 写明：FE 只碰 `frontend/`；BE 只碰 `backend/`（+ 约定包路径） | 每个并行 phase **立即** |
| **L1 WM 沙箱** | `working-memory/agents/fe/` · `be/` 放该 agent 的 `loop.md` / 笔记（**不**放业务源码） | 下一次并行起强制 |
| **L2 git** | `wip/pN-fe-*` / `wip/pN-be-*`；父会话 merge | 已有分支约定时强制 |
| **L3 worktree** | 每**写者**一棵工作树（或工具链等价隔离 checkout） | **≥2 写者并行时默认**（见 [[patterns/extreme-unattended-cluster]] v8）；不再等「冲突频繁再上」 |

### 分仓（权限分离）时

| 协同面 | 做法 |
| :--- | :--- |
| **契约** | 独立 `contracts` / OpenAPI 仓或包；FE/BE **只依赖生成物**，禁止直读对方源码 |
| **编排 WM** | `loop.md` 放**中立仓**（可写双方的编排角色）；业务细节仍在各自仓 WM |
| **Agent 权限** | brief 写死 remote/路径写权限；契约变更走第三仓 PR |
| **验收** | 父（或 CI）跑契约 diff + 各仓目标测；「两边各自绿」≠ 联调通过 |

拆 FE/BE **仓**的门槛：独立发布 / 权限 / 团队。未达门槛 → 用 monorepo 三级，不拆远程。

### 派出时必落盘（与投递凭据对齐）

```text
working-memory/agents/<role>/
  loop.md       ← 范围、禁区、验收、停条件
  status.md     ← 进行中 / 阻塞 / 完成（含 HEAD + Tests run）
```

任务级 brief 仍可放在 `working-memory/tasks/<task>/`（见 [[patterns/parallel-work-needs-delivery-proof]]）。

### 加厚切片（与 extreme v4 对齐）

并行派出时，**每个**子代理一次交付：

1. 路径沙箱内的代码  
2. **目标测**命令与绿证（写进 `status.md`）  
3. 新接通则 RUNBOOK/契约 **一行**  
4. `status.md` 回填 HEAD  

禁止：只交 UI stub / 只交 Controller 无测 / 「下 tick 再补 RUNBOOK」。

### 目标测

- 默认：`*ControllerTest`、单 IT、`tsc --noEmit` 等与切片相关命令  
- 全量 `mvn test` / 全仓 e2e：仅在发布门禁或父显式要求时  

## 反面

- 不要在 `frontend/` 里建第二份 `working-memory/` 当知识库。
- 不要让 FE agent 改 `backend/src/main/java`（除非 brief 显式授权且单文件契约）。
- 不要用「再派一个 agent」代替写清路径边界。
- 不要分仓后仍让 FE 克隆 BE 源码当「类型来源」。
- 不要把半截岛当成一次集群交付。

## 关联

[[patterns/parallel-work-needs-delivery-proof]] [[patterns/pressure-routing]] [[patterns/extreme-unattended-cluster]] [[W10-working-memory]] [[cli-agent-boundaries]] [[patterns/shared-kernel-across-bc]]
