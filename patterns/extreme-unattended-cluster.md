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
trigger: 无人托管要极端集群；压测 collaboration 却只涨测试/validate；或子代理卡住无人接管；或集群空转/推进过慢；或要用业务仓双轴压 KB
provenance: 2026-09-17 极端化→v2 双轴/集群/≤300ms→v3 双证明+父验绿→v4 吞吐优化（单主轴/加厚切片/目标测/WM 单真源）
---

# 极端无人托管集群：每 tick 门禁

## 上下文

一人长期离席、多 agent（父 + 子）在 wake 循环里压测或推进。
没有人盯会话时，**误路由与静默失败会被放大**：测试数涨、子代理空转、KB 不动，却仍被当成「在进化」。
门禁过严或切片过碎时，**正确性在、吞吐崩**——看起来像「推进很慢」。

本模式是 [[patterns/pressure-routing]] 在**极端无人托管 + 集群**下的硬门禁；工作区划界见 [[S36]]，账本落点见 [[patterns/project-evidence-vs-kb-ledger]]。

## 问题

- **层口号 vs 落点**：任务名写「collaboration 压测」，每 tick 只动消费者仓 / 业务仓。
- **静默子代理**：超时无回执时父会话继续 wake，空转被间隔掩盖。
- **无增量不收口**：连续 tick 无 KB / 无候选 interceptions，仍降间隔硬跑。
- **账本污染**：把轮次报告写进 `evolution-log`，代谢配额从未触发。
- **吞吐塌方**：每 tick 强绑双轴 + 半截岛多次往返 + 全量 `mvn test` + WM 双仓同写 → 墙时间爆炸。

## 方案

### 每 tick 强制自问（第一步）

> **本 tick 我期望哪个仓库的 HEAD 发生变化？**（可多个，须列全）
> **本 tick 主轴是 L2 还是 L3？**（默认**单主轴**；双轴成功另开声明）

答完再动手。未写入 `loop.md` / `status.md` 的期望 = 未声明 = 本 tick 无效。

### 集群门禁表（v4 · 极端 + 吞吐）

| 门禁 | 规则 | 违反时 |
| :--- | :--- | :--- |
| **期望 HEAD** | 每 tick 显式声明目标仓（可多仓） | 停本 tick；改声明或改层 |
| **单主轴优先** | 默认每 tick **只交 L2 或只交 L3**；双轴成功须**显式声明**且满足「同 tick 双证明」 | 未声明却自称双轴 → 记 idle |
| **双轴（evo×KB）** | 「用 evolutionary 压 collaboration」→ 任务级须覆盖两轴；**不必每 tick 两轴都动** | 连续 N tick 两轴均无增量 → 停 |
| **L3 成功判据** | 自称 L3 / KB 压测 → **必须**有 `collaboration` **条目 diff** 或主表 `interceptions`/`known-gaps` **+1 行**；仅 validate / CLI 绿 **不算** | 不得开下一 wake；W4 |
| **加厚切片** | 可并行派出时，每半边 brief 须一次交付：**代码 + 目标测绿 + RUNBOOK/契约一行 + `agents/*/status.md`**；禁止「只交半截岛 / 只交 stub」 | 本 tick 无效；父拒收或接管补齐 |
| **强制集群** | 有可并行切片时 **≥2 子代理**（典型 FE∥BE）；单代理只做「不可再拆」的收口 tick | 本 tick 无效；重派 |
| **子代理超时** | 外部硬超时默认 **≤60s**；无回执 → **父立刻接管**（续做/重派/收窄），禁止干等下一 wake | 见 [[patterns/parallel-work-needs-delivery-proof]] |
| **目标测** | 收口验绿默认跑**切片相关**测试（如 `*ControllerTest` / 单 IT）；禁止无理由全量 `mvn test` / 全仓 e2e | 记摩擦；下 tick 改目标测 |
| **WM 单真源** | 任务 `loop.md` **真源只写业务仓**（如 evolutionary）；编排仓（collab-cli）仅**指针**，可批量/收口 tick 更新，禁止每微提交双写 | 双写冲突 → 以业务仓为准 |
| **连续空转** | 极端默认 **N=3**（双轴任务：两轴均无声明增量才计 idle）→ **停 wake** | 收口 + W4 |
| **wake 上限** | 无人托管动态 wake **≤300ms**（心跳）；禁止为「看起来在跑」拉长间隔 | 降回 ≤300ms 或停 |
| **代谢配额** | 新增条目达阈 → 先处理再堆；「暂不入库」**不算**产出 | 见 [[meta/pruning-policy]] |
| **工作区** | 遵守 [[S36]]；越权 diff **拒收** | 父合并前拒收 |
| **同 tick 双证明** | **仅当**本 tick 宣称双轴成功：须同 tick 留下 **evolutionary commit** 与 **collaboration 条目/主表 diff** | 不得宣称双轴完成；改单主轴或记 idle |
| **父验绿** | 合并/收口前须有测绿证据（目标测日志或子代理 status 写明 Tests run） | 拒收无证据 HEAD 登记 |
| **账本** | 轮次报告只进业务仓 WM；KB 只记本体里程碑 / 主表一行 | 见 [[patterns/project-evidence-vs-kb-ledger]] |

### 加厚 brief 最小清单（写入子代理任务）

```text
范围路径（沙箱）
验收：HTTP/UI 行为 + 目标测命令
交付物：代码 commit + status.md（含 HEAD 与 Tests run）
RUNBOOK 或契约一行（若新接通）
禁区：对方目录 / push / merge version/*
超时：父 60s 接管
```

### 与既有条目的分工

| 条目 | 管什么 | 本模式补什么 |
| :--- | :--- | :--- |
| [[patterns/pressure-routing]] | L1/L2/L3 选层与成功判据 | 无人托管下**强制执行**门禁；单主轴优先 |
| [[S36]] | 多 agent 路径 / WM 沙箱 / 分仓协同 | 极端模式下违规 diff **拒收**；加厚切片落 brief |
| [[patterns/project-evidence-vs-kb-ledger]] | 证据 vs 账本分层 | 长运行禁止把日记灌进 KB；WM 单真源 |
| [[patterns/parallel-work-needs-delivery-proof]] | 投递凭据与超时观察 | 超时后**父必须接管**；加厚 = 一次可验收投递 |

### 启动 checklist（写入父 `loop.md` 首段）

1. 本 tick **主轴**（L2 或 L3）与期望 HEAD  
2. **≥2 子代理** 时：加厚 brief + 路径沙箱  
3. 子代理硬超时（≤60s）与父接管动作  
4. 目标测命令（写进 loop，禁止默默全量）  
5. 空转停 wake 的 N（默认 3）与 wake≤300ms  
6. 代谢触发时先修剪再继续  

## 反面

- 不要在无人托管时把「wake 还在跳」当成进度。
- 不要把子代理自述或 validate 归零当成 L3 产出。
- 不要把「业务仓测数涨」当成压到了 collaboration。
- 不要在可并行时故意单代理「串行假装托管」。
- 不要在子代理超时后只「再派一个」而不接管未完成产物。
- 不要为维持极端节奏而绕过代谢配额。
- 不要每 tick 强绑双轴 commit（未撞墙也 harvest）——那是制造空转。
- 不要派「只写 panel stub、测与 RUNBOOK 下 tick 再说」的半截切片。
- 不要无理由全量回归当收口证明。
- 不要每微提交同时改 evolutionary loop 与 collab-cli 指针。

## 关联

[[patterns/pressure-routing]] [[patterns/project-evidence-vs-kb-ledger]] [[S36]] [[patterns/parallel-work-needs-delivery-proof]] [[patterns/waiting-is-a-decision-window]] [[meta/pruning-policy]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[W10-working-memory]]
