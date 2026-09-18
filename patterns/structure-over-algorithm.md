---
id: structure-over-algorithm
type: pattern
status: active
source: "计算机科学（Wirth: 程序 = 数据结构 + 算法）"
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [structure-over-algorithm]
trigger: 复杂逻辑第一反应是把算法写聪明；或在错误结构上继续打补丁；或 if-else/状态变量越堆越长
enforced: null
---
# 结构优先
## 上下文
面对复杂逻辑，习惯直接想"算法"。
## 问题

遇到复杂逻辑，第一反应是"把算法写得更聪明"，而不是问"结构是不是错了"——于是在错误的结构上继续打补丁。

## 方案
- **先问"数据结构对不对"，再问"算法怎么写"**。
- 结构对了，算法自然简化；结构错了，算法再优也是负收益。
### 应用
| 场景 | 算法思维 | 结构思维 |
| :--- | :--- | :--- |
| 复杂条件判断 | 更长的 if-else | 策略模式/表驱动 |
| 状态管理 | 更多变量追踪 | 状态机 |
| 分层架构 | 每层写更多检查 | 依赖方向约束 |
| 团队协作 | 靠约定和 review | 靠目录结构和 ESLint |
## 反面
- 不要否定算法——结构优先，不代表算法不重要。
- 不要用"结构"作为不写算法细节的借口。
## 关联
[[S17-ESLint-工具约束]] [[patterns/allowlist-over-denylist]]
