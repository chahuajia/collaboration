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
trigger: 无人托管要极端集群；压测 collaboration 却只涨测试/validate；或子代理卡住无人接管；或要用业务仓双轴压 KB
provenance: 2026-09-17 用户要求「更加极端化」；同日二次加码：双轴 + 强制集群 + ≤300ms wake
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

### 集群门禁表（v2 · 更极端）

| 门禁 | 规则 | 违反时 |
| :--- | :--- | :--- |
| **期望 HEAD** | 每 tick 显式声明目标仓（可多仓） | 停本 tick；改声明或改层 |
| **双轴（evo×KB）** | 「用 evolutionary 压 collaboration」→ 每 tick 须声明 **L2=`evolutionary` 与/或 L3=`collaboration`**；只涨业务测数、KB 零 harvest **不算**双轴成功 | 记 idle；连续 N 停 |
| **L3 成功判据** | 自称 L3 / KB 压测 → **必须**有 `collaboration` **条目 diff** 或主表 `interceptions`/`known-gaps` **+1 行**；仅 validate / CLI 绿 **不算** | 不得开下一 wake；W4 |
| **强制集群** | 有可并行切片时 **≥2 子代理**（典型 FE∥BE）；单代理只做「不可再拆」的收口 tick | 本 tick 无效；重派 |
| **子代理超时** | 外部硬超时默认 **≤60s**；无回执 → **父接管**（续做/重派/收窄），禁止干等下一 wake | 见 [[patterns/parallel-work-needs-delivery-proof]] |
| **连续空转** | 极端默认 **N=3**（双轴：两轴均无声明增量才计 idle）→ **停 wake** | 收口 + W4 |
| **wake 上限** | 无人托管动态 wake **≤300ms**（心跳）；禁止为「看起来在跑」拉长间隔 | 降回 ≤300ms 或停 |
| **代谢配额** | 新增条目达阈 → 先处理再堆；「暂不入库」**不算**产出 | 见 [[meta/pruning-policy]] |
| **工作区** | 遵守 [[S36]]；越权 diff **拒收** | 父合并前拒收 |
| **账本** | 轮次报告只进业务仓 WM；KB 只记本体里程碑 / 主表一行 | 见 [[patterns/project-evidence-vs-kb-ledger]] |

### 与既有条目的分工

| 条目 | 管什么 | 本模式补什么 |
| :--- | :--- | :--- |
| [[patterns/pressure-routing]] | L1/L2/L3 选层与成功判据 | 无人托管下**强制执行**门禁，不允许「看起来在跑」 |
| [[S36]] | 多 agent 路径 / WM 沙箱 | 极端模式下违规 diff **拒收** |
| [[patterns/project-evidence-vs-kb-ledger]] | 证据 vs 账本分层 | 长运行禁止把日记灌进 KB |
| [[patterns/parallel-work-needs-delivery-proof]] | 投递凭据与超时观察 | 超时后**父必须接管**，不得空等下一 wake |

### 启动 checklist（写入父 `loop.md` 首段）

1. 压力层与期望 HEAD（双轴时两仓都写）  
2. **≥2 子代理** 切片表 + 各自路径沙箱  
3. 子代理硬超时（≤60s）与父接管动作  
4. 空转停 wake 的 N（默认 3）与 wake≤300ms  
5. 代谢触发时先修剪再继续  

## 反面

- 不要在无人托管时把「wake 还在跳」当成进度。
- 不要把子代理自述或 validate 归零当成 L3 产出。
- 不要把「业务仓测数涨」当成压到了 collaboration。
- 不要在可并行时故意单代理「串行假装托管」。
- 不要在子代理超时后只「再派一个」而不接管未完成产物。
- 不要为维持极端节奏而绕过代谢配额。

## 关联

[[patterns/pressure-routing]] [[patterns/project-evidence-vs-kb-ledger]] [[S36]] [[patterns/parallel-work-needs-delivery-proof]] [[patterns/waiting-is-a-decision-window]] [[meta/pruning-policy]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[W10-working-memory]]
