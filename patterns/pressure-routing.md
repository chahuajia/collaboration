---
id: pressure-routing
type: pattern
status: active
created: 2026-09-17
updated: 2026-09-17
author: heiniao
source: 人机协作实践（压测路由误判）
aliases:
  - pressure-routing
  - P-pressure
trigger: 压测放哪一层；长运行空转；测数涨但 KB 不涨；不确定本 tick 该变哪个仓的 HEAD
provenance: collab-pressure 17 轮；2026-09-17 极端集群补强交叉引用
enforced: null
---

# 压测路由：三层压力与成功判据

## 上下文

「用复杂项目压 COLLABORATION」是合理目标，但**压力落在哪一层**决定**产物应该出现在哪个仓库**。
把 L1 工具链压测当成 L3 KB 演化压测，会长时间空转：测试数涨、CLI 更稳，**条目与 interceptions 不涨**。

## 问题

- **仓库错位**：在 `collab-cli` 里跑 MCP/validate/index 链，期望 `collaboration` 条目 diff —— 目标与落点不一致。
- **成功判据漂移**：用「测试绿 / 轮次完成」代替「KB 有 harvest」；17 轮后才发现偏了。
- **长运行放大误判**：one-shot wake 每 tick 有增量（新测试），看起来像「在进化」，实则**代谢配额从未触发**。

## 方案

### 三层压力模型

| 层 | 名称 | 落点仓库 | 典型动作 | 成功判据（本 tick） |
| :-- | :--- | :--- | :--- | :--- |
| **L1** | 工具链压测 | 消费者仓（如 `collab-cli`） | CLI/MCP/validate/index/commit 回归 | 测试绿；**不要求** `collaboration` HEAD 变 |
| **L2** | 业务/spec 压测 | 业务仓 WM + 实现（如 `evolutionary`） | 规格、契约、AC、领域实现撞设计墙 | 规格/契约/代码 diff；**interceptions 候选**记入业务仓 WM |
| **L3** | KB 演化压测 | **`collaboration`** | 实现或复盘暴露缺口 → W4/W5 harvest 新/改条目 | **`collaboration` 工作区有条目 diff** 或 `interceptions` 计数增加 |

**流向**：L2 撞墙 → harvest 进 L3；L1 只保证工具能服务 L2/L3，**不替代** L3。

**账本落点**：项目轮次/phase 报告**不得**写入 `meta/evolution-log` 正文；L2 拦截先记业务仓候选。详见 [[patterns/project-evidence-vs-kb-ledger]]。

**极端无人托管**：并行 ≥2、超时父接管、L3 连续 3 tick 无增量即停。详见 [[patterns/extreme-unattended-cluster]]。

### 每 tick 门禁（写进 `loop.md` 首段）

长运行任务启动前，loop 必须声明：

1. **压力层**（L1 / L2 / L3）
2. **本 tick 期望变 HEAD 的仓库**（可多个，须列全）
3. **无增量时的停止条件**（停 wake，不空转）

**强制自问**（每 tick 第一步）：

> **本 tick 我期望哪个仓库的 HEAD 发生变化？**

- 答「只有 collab-cli」→ 这是 **L1**，不要称「KB 压测 / collaboration 演化」。
- 答「collaboration」→ 必须有 **W5 路径**（新条/改条/interceptions）；仅 validate 归零不算演化。
- 答「evolutionary 实现 + 可能 collaboration」→ **L2 为主**；KB diff 只在 interception 成立时发生。

### 选层决策（30 秒）

| 你的目标 | 选层 | 读 |
| :-- | :--- | :--- |
| CLI/MCP 别回归 | L1 | [[patterns/reproducible-verification]] |
| 复杂业务逼出设计/条目 | L2 → L3 | [[A10-review-前置原则]] → [[W4-three-question-retro]] → [[W5-update-collaboration]] |
| 直接补 KB 缺口 | L3 | [[meta/pruning-policy]]（能说出拦住了什么） |

### 收口信号

以下任一出现，**停止长运行**并 W4 复盘，而不是开下一轮：

- L1：连续 **3 tick** 无新失败模式，只有同类测试堆叠
- L2：当前 phase 规格/契约已收口，下一动作是「实现」而非「再写 spec」
- L3：连续 **5 tick** `interceptions` 与条目 diff 均为 0（代谢配额：暂不入库 不算产出）

## 反面

- 不要把「collaboration validate 113/0」当成 KB 演化成果 —— 那是 L1 门禁，不是 L3 产出。
- 不要用「再加一层压测仪式」代替改正确的仓 —— 见 [[patterns/policy-without-mechanism]]。
- 不要在 L1 任务名里写「Collaboration 复杂场景压测」却不改 collaboration 条目。
- 不要用 wake 间隔掩盖「无 KB 增量」—— **无增量就停**，不要降间隔硬跑。
- 不要把 L2 的规格堆叠（61 AC、0 行业务代码）当成「已经压过 KB」—— 规格在 WM，条目在 collaboration。

## 关联

[[patterns/evolution-loop]] [[patterns/distributed-evolution]] [[patterns/layer-vs-context]] [[patterns/reproducible-verification]] [[patterns/project-evidence-vs-kb-ledger]] [[A10-review-前置原则]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[meta/pruning-policy]]
