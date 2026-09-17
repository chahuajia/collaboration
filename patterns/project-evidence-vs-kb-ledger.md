---
id: project-evidence-vs-kb-ledger
type: pattern
status: active
created: 2026-09-17
updated: 2026-09-17
author: heiniao
source: 人机协作实践（账本被项目日志淹没）
aliases:
  - project-evidence-vs-kb-ledger
  - P-ledger
provenance: evolutionary 0→7 复盘——meta/evolution-log 与 interceptions 行级证据几乎全绑换电压测，读起来像项目日记进了公共库
---

# 项目证据 vs KB 账本

## 上下文

`collaboration` 需要可审计的演化与拦截信号；业务仓会产生**巨量**轮次报告、phase 复盘、测试日志。
把两者写进同一主文件，长期知识被过程噪音淹没，检索与修剪同时失效。

## 问题

- `meta/evolution-log` 变成项目运行日记，KB 版本里程碑被淹没。
- `interceptions` / `known-gaps` 主表塞满单仓证据路径，跨项目不可扫。
- Agent 分不清「该写 collaboration」还是「该写业务仓 WM」。

## 方案

### 分层落点

| 层 | 放哪 | 写什么 | 体量 |
| :--- | :--- | :--- | :--- |
| **KB 账本** | `collaboration/meta/evolution-log.md` | 仅 **KB 本体**版本/结构/路由/代谢事件 | 短；一行一里程碑 |
| **KB 判决索引** | `interceptions.md` / `known-gaps.md` 主表 | 跨项目可检索：**条目 · 一句话 · 链接** | 行级摘要，不贴报告正文 |
| **项目证据** | 业务仓 `working-memory/` 或 `specs/` | 轮次报告、phase retro、AC 明细 | 可巨量 |
| **候选桥** | 业务仓 `interceptions-candidates.md` | L2 撞墙未 harvest | 阶段内；W4 后升主表一行 |

### 硬规则

1. **禁止**把项目轮次/phase 过程日志整段写入 `evolution-log` 正文。
2. `evolution-log` 若需提及项目：一行摘要 + **链接**到业务仓证据路径。
3. L2 拦截先记业务仓候选；**仅** W4 通过后 harvest **一行**进 `interceptions`。
4. 工具债（catalog trigger、CLI）→ `known-gaps`；业务缺口候选 → 业务仓，达标再入库。
5. 主 log / 主表超阈值 → 归档旧段到 `meta/archive/`，索引保留可达。

### 与压测层的关系

| 压力层 | 默认写 |
| :--- | :--- |
| L1 | 消费者仓 WM / 测试 |
| L2 | 业务仓 WM；候选 interceptions |
| L3 | collaboration 条目 diff 或主表 +1 行 |

详见 [[patterns/pressure-routing]]。

## 反面

- 不要为「方便 AI 下次接着写」把报告贴进 collaboration。
- 不要删项目证据——迁出到业务仓，不是销毁。
- 不要用 `evolution-log` 代替 W10 工作记忆。

## 关联

[[patterns/pressure-routing]] [[patterns/three-layer-memory]] [[W10-working-memory]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[meta/pruning-policy]] [[meta/interceptions]] [[meta/known-gaps]]
