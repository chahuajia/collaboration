---
id: dependency-decision
type: pattern
status: active
source: 供应链管理（Make-or-Buy）
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [dependency-decision]
trigger: 不确定要不要引入依赖 / 放哪一层；或差点把框架塞进领域
enforced: null
---

# 依赖决策三问 + 位置三问

## 上下文

需要引入新依赖或放置新文件。

## 问题

引入依赖靠"最佳实践""以后可能用得上"来决策：成本没人算，位置没人判——最后依赖方向被破坏（领域层 import 第三方）。

## 方案

### 引入三问

1. **问题**：没有它，我会遇到什么具体问题？
2. **收益**：收益有多大？能否量化？
3. **成本**：学习、升级、体积、锁定、攻击面。

**三问答不上来 → 不加**。

### 位置三问

1. **它属于哪一层？**（领域 / 应用 / 基础设施 / CLI）
2. **它会泄漏到其他层吗？**（如领域层 import zod）
3. **如果替换它，要改多少地方？**

**领域层永远不 import 第三方**。

### 设计决策姊妹条款

**每个设计决策都要能说出"没有它，我会遇到什么具体问题"。**

## 反面

- 不要用"最佳实践"作为理由。
- 不要用"以后可能"作为理由。
- 不要为了"统一"牺牲"语义准确"。

## 关联

[[patterns/design-decision]] [[S19-Make-or-Buy]] [[patterns/allowlist-over-denylist]]
