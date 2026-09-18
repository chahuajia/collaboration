---
id: policy-without-mechanism
type: pattern
status: active
created: 2026-09-18
updated: 2026-09-18
source: 人机协作实践（extreme v5–v7 政策堆砌负吞吐）+ 外源对照（隔离/回执/会计）
author: heiniao
aliases:
  - policy-without-mechanism
  - governance-theater
  - P-neg-opt
trigger: 加了规则/版本/门禁却更慢；优化像负优化；政策很多基础设施没有；docs 与派工热闹 feat 不涨
provenance: 2026-09-18 evo-collab-extreme——条文迭代快于 worktree/回执/会计落地，集群净效率低于单主轴
enforced: null
---

# 无机制的政策是负优化

## 上下文

协作库与集群编排都会「加门禁」：新版本、新 checklist、新反面句。
当**可观察机制**（隔离、超时强制接管、回执、会计、测绿凭证）未同步落地时，新增政策只增加**协调税与假进度面**，吞吐下降——这叫**负优化**，不是「还没优化够」。

本模式回答：**什么时候该停写条文、改装管道？**

## 问题

- **政策通胀**：同一模式 vN→vN+1 只加表行，不改执行面（同树多写、空 transcript、docs 当 HEAD）。
- **活动≠产出**：wake / agent id / `docs(wm)` / validate 绿被当成进度。
- **优化错层**：该上基础设施（worktree、Return contract、派出会计）时却只加「不要…」反面句。
- **负反馈延迟**：墙钟变长、feat/docs 比恶化，却仍宣称「已按最新门禁执行」。

## 方案

### 判定三问（任一「否」→ 停写政策，先装机制）

1. **本条规则的违反，能否在 ≤60s 内被外部观察发现？**（文件 / transcript / git / 测绿）
2. **发现后是否有强制动作？**（接管 / 拒收 / 停 wake / 改串行——不是「记一笔」）
3. **执行面是否已具备？**（worktree、回执模板、会计字段、目标测命令）——**没有则本条先不入库或标 `needs-mechanism`**

### 政策 vs 机制对照

| 只写政策（负优化风险） | 须同 tick 落地的机制 |
| :--- | :--- |
| 「超时要接管」 | 探针 + 父写代码/重派的硬截止 |
| 「不要同文件双写」 | **worktree / 分支隔离**（[[S36]] L3） |
| 「要有交付证明」 | **feat commit + Tests run**；禁派出专用 docs |
| 「派了要对账」 | loop 头 `dispatched=N recovered=M` |
| 「沉默不算进行中」 | 禁预写 🔄；空 transcript = FAILED |
| 「L3 要动 KB」 | collaboration **条目/主表 diff**（[[patterns/pressure-routing]]） |
| 「别把日记写进账本」 | 分层落点（[[patterns/project-evidence-vs-kb-ledger]]） |
| 「要修剪」 | 可达性扫描/除名动作，而非从未执行的时间规则 |

### 入库门禁（防再堆 v9 空谈）

新增或大改 **流程/集群类**条目时，`evolution-log` 同行须含至少一项：

- 新**可执行产物**（模板、脚本、loop 字段、worktree 约定），或  
- **关闭**一条「有政策无机制」的 [[known-gaps]]，或  
- 显式写 `needs-mechanism: <缺什么>` 且**不**宣称已生效

### 与既有条目

| 条目 | 关系 |
| :--- | :--- |
| [[patterns/extreme-unattended-cluster]] | v8 起把隔离/回执/会计升为机制；本模式解释 **为何 v5–v7 曾负优化** |
| [[patterns/parallel-work-needs-delivery-proof]] | 「派出≠送达」的实例；本模式升到**政策层元规则** |
| [[patterns/pressure-routing]] | L1 绿当 L3 = 活动≠产出 |
| [[patterns/project-evidence-vs-kb-ledger]] | 日记进账本 = 假繁荣 |
| [[patterns/entropy-reduction]] / [[meta/pruning-policy]] | 「有修剪政策从未测量」= 同构负优化 |
| [[patterns/waiting-is-a-decision-window]] | 边界不清时加 agent = 负优化 |

## 反面

- 不要用「再写一版更严的门禁」代替 worktree / 回执 / 接管。
- 不要在 `needs-mechanism` 未清时把条目 trigger 写进 `AGENTS.md` 症状表冒充已生效。
- 不要把 evolution-log 的版本号递增当成协作能力提升。

## 关联

[[patterns/extreme-unattended-cluster]] [[patterns/parallel-work-needs-delivery-proof]] [[patterns/pressure-routing]] [[patterns/project-evidence-vs-kb-ledger]] [[S36]] [[meta/pruning-policy]] [[patterns/entropy-reduction]] [[patterns/waiting-is-a-decision-window]] [[W4-three-question-retro]]
