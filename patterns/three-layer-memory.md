---
id: three-layer-memory
type: pattern
status: active
source: 认知心理学（工作记忆 / 长期记忆）+ 数据库（缓存 / 主存储 / 冷存储）
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [three-layer-memory]
trigger: 设计 AI/系统知识存储却只问"上下文够不够"；或想把一切塞进上下文/全扔进知识库
enforced: null
---
# 三层记忆
## 上下文
需要设计"AI 或系统的知识存储"——直觉上会问"上下文够不够"。
## 问题
- 直觉解法是"把所有东西放进上下文"或"寄放到知识库"——都是极端。
- 缺少"临时 vs 长期"的分层。
## 方案
### 三层结构
| 层 | 承载 | 生命周期 | 存储位置 | 读频率 |
| :--- | :--- | :--- | :--- | :--- |
| **工作记忆** | 当前任务状态 | 小时-天 | `working-memory/` | 高 |
| **进度文档** | 跨对话工作状态 | 天-周 | 同上（含归档） | 中 |
| **长期知识** | 提炼后的条目 | 月-年 | `COLLABORATION/` | 低 |

账本（evolution-log / interceptions）如何避免被项目证据淹没 → [[patterns/project-evidence-vs-kb-ledger]]。
### 核心原则
- **不同层有不同的"写频率"**：工作记忆频繁重写；长期知识谨慎添加。
- **不同层有不同的"归档策略"**：工作记忆自动归档；长期知识按 pruning-policy。
- **不同层有不同的"权威性"**：长期知识是"共识"；工作记忆是"当前状态"。
### 数据流
```text
任务进行中 → 工作记忆（快速读写）
   ↓ 阶段完成
归档快照 → _archive/（历史记录）
   ↓ 提炼
COLLABORATION 条目（长期知识）
```
### 关键判据

|内容|归属|
|---|---|
|"当前在第 3 步"|工作记忆|
|"上次决定用方案 A"|工作记忆（若未稳定）/ COLLABORATION（若已稳定）|
|"分层依赖用白名单"|COLLABORATION|
|"今天跑测试 3 次失败"|**不进任何库**——临时噪音|

## 反面

- 不要把所有内容都"入库"——**大部分上下文不值得保存**。
    
- 不要跨层——工作记忆里不放"共识"，COLLABORATION 里不放"临时状态"。
    
- 不要用"信息完整"作为"保存一切"的借口——**熵增**比**信息丢失**更糟。
    

## 关联

[[W10-working-memory]] [[patterns/context-overflow-solved-by-retrieval]] [[meta/pruning-policy]]
