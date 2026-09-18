---
id: derivation-over-copy
type: pattern
status: active
source: 数据库规范化 + 供应链管理（派生 vs 复制）
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [derivation-over-copy]
trigger: 多处手写同一组默认值/目录；或改一处忘了另一处；或觉得复制成本为零
enforced: null
---

# 派生优于复制

## 上下文

需要在多处表达"同一个值集合"（如"5 个目录"、"默认值"）。

## 问题

- **复制**：两处独立定义同一集合 —— **必然漂移**。
- **"复制成本 = 0"是幻觉** —— 未来同步成本是**每次**。
- **"派生成本 = 1 行 import"** —— 但**未来同步成本 = 0**。

## 方案

### 一、判据："它是源头还是派生？"

| 类型 | 例子 | 处理 |
| :--- | :--- | :--- |
| **源头** | `EntryKindDir` | 定义一次 |
| **派生** | `DEFAULT_DIRS` | `Object.values(EntryKindDir)` |

### 二、能派生就派生

```ts
// ❌ 复制
const DEFAULT_DIRS = ['skills', 'agreements', ...];

// ✅ 派生
const DEFAULT_DIRS = Object.values(EntryKindDir);
```
**即使代价是"多一次 import"** —— **也值得**。

### 三、派生的"传染链"

**派生会传染**：

- **A 派生自 B** —— B 变了，A 自动变。
    
- **B 又派生自 C** —— C 变了，B 和 A 都变。
    
- **结果**：**改源头一处** —— **全链跟随**。
    

### 四、例外

**当"派生"违反其他标准时** —— 才允许硬编码 + 注释。

**例子**：`scripts/*.mjs` 如果"必须独立运行" —— 可以硬编码 —— **但要注释"需和 X 同步"**。

**但**：**"独立运行"通常不是必要的** —— 它是"想象的约束"。

## 反面

- 不要为了"少一次 import"而复制。
    
- 不要让两处定义同一集合。
    
- 不要把"独立运行"当"硬编码"的借口。
    

## 关联

[[S15-TS-类型工厂]] [[S32-数据变更管理原则]] [[patterns/layer-vs-context]]
