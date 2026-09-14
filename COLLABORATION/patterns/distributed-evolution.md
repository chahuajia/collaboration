---
id: distributed-evolution
type: pattern
status: active
source: 分布式系统 + 生物进化
---
# 分布式演化
## 上下文
社区规范需要多人多地并行演化，不能依赖中心协调。
## 问题
- 中心化主干容易成为瓶颈。
- 完全去中心化又难以收敛。
## 方案
- **主干（main）**：稳定的共识基线。
- **Fork**：个人/团队的变异空间。
- **PR**：变异进入主干的通道。
- **CODEOWNERS**：承重墙的保护者。
- **Profile**：个人侧重的声明。
- 结果：**变异在边缘，选择在主干，遗传靠 PR**。
## 反面
- 不要让主干频繁变动。
- 不要让 fork 永久漂移。
- 不要用投票替代讨论。
## 关联
[[A7]] [[W6]] [[S11]] [[patterns/evolution-loop]] [[patterns/horizontal-gene-transfer]]