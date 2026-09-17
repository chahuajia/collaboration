---
id: extreme-unattended-cluster
type: pattern
status: active
created: 2026-09-17
updated: 2026-09-17
source: 人机协作实践（极端无人托管 + 多 agent 集群）
author: heiniao
aliases:
  - extreme-unattended-cluster
  - P-extreme-cluster
provenance: 2026-09-17 用户要求「更加极端化」压测 collaboration
---

# 极端无人托管集群：每 tick 门禁

## 上下文

一人长期离席、多 agent（父 + 子）在 wake 循环里压测或推进。
没有人盯会话时，**误路由与静默失败会被放大**：测试数涨、子代理空转、KB 不动，却仍被当成「在进化」。

本模式是 [[patterns/pressure-routing]] 在**极端无人托管 + 集群**下的硬门禁；工作区划界见 [[S36]]，账本落点见 [[patterns/project-evidence-vs-kb-ledger]]。

## 问题

- **层口号 vs 落点**：任务名写「collaboration 压测」，每 tick 只动消费者仓 / 业务仓。
- **静默子代理**：超时无回执时父会话继续 wake，空转被间隔掩盖。
- **无增量不收口**：连续 tick 无 KB / 无候选 interceptions，仍降间隔硬跑。
- **账本污染**：把轮次报告写进 `evolution-log`，代谢配额从未触发。

## 方案

### 每 tick 强制自问（第一步）

> **本 tick 我期望哪个仓库的 HEAD 发生变化？**（可多个，须列全）

答完再动手。未写入 `loop.md` / `status.md` 的期望 = 未声明 = 本 tick 无效。

### 集群门禁表

| 门禁 | 规则 | 违反时 |
| :--- | :--- | :--- |
| **期望 HEAD** | 每 tick 显式声明目标仓 | 停本 tick；改声明或改层 |
| **L3 成功判据** | 自称 L3 / KB 压测 → **必须**有 `collaboration` **条目 diff**（新/改 pattern·skill·workflow 等）或主表 `interceptions`/`known-gaps` **+1 行**；仅 validate 归零 / 仅 CLI 绿 **不算** | 不得开下一 wake；W4 复盘 |
| **子代理超时** | 派出时写死外部超时；`status`/`heartbeat` 停更或无回执 → **父接管**（续做 / 重派 / 收窄），禁止干等 | 见 [[patterns/parallel-work-needs-delivery-proof]] |
| **连续空转** | 连续 N tick（极端 L3 默认：**3**，严于 [[patterns/pressure-routing]] 基线 5）期望仓无声明增量 → **停 wake**，不降间隔硬跑 | 收口 + W4 |
| **代谢配额** | 新增条目达阈 → 先处理（dormant/合并/修剪）再继续堆；「暂不入库」**不算**产出 | 见 [[meta/pruning-policy]] |
| **工作区** | FE/BE 等角色遵守 [[S36]] 路径与 `wm/agents/{role}`；不越权抢文件 | 父合并前拒收越权 diff |
| **账本** | 项目轮次/phase 报告只进业务仓 WM；KB 只记本体里程碑与主表一行 | 见 [[patterns/project-evidence-vs-kb-ledger]] |

### 与既有条目的分工

| 条目 | 管什么 | 本模式补什么 |
| :--- | :--- | :--- |
| [[patterns/pressure-routing]] | L1/L2/L3 选层与成功判据 | 无人托管下**强制执行**门禁，不允许「看起来在跑」 |
| [[S36]] | 多 agent 路径 / WM 沙箱 | 极端模式下违规 diff **拒收** |
| [[patterns/project-evidence-vs-kb-ledger]] | 证据 vs 账本分层 | 长运行禁止把日记灌进 KB |
| [[patterns/parallel-work-needs-delivery-proof]] | 投递凭据与超时观察 | 超时后**父必须接管**，不得空等下一 wake |

### 启动 checklist（写入父 `loop.md` 首段）

1. 压力层（L1 / L2 / L3）与期望 HEAD 仓列表  
2. 子代理超时预算与接管动作  
3. 空转停 wake 的 N  
4. 代谢触发时先修剪再继续  

## 反面

- 不要在无人托管时把「wake 还在跳」当成进度。
- 不要把子代理自述或 validate 归零当成 L3 产出。
- 不要在子代理超时后只「再派一个」而不接管未完成产物。
- 不要为维持极端节奏而绕过代谢配额。

## 关联

[[patterns/pressure-routing]] [[patterns/project-evidence-vs-kb-ledger]] [[S36]] [[patterns/parallel-work-needs-delivery-proof]] [[patterns/waiting-is-a-decision-window]] [[meta/pruning-policy]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[W10-working-memory]]
