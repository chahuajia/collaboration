---
id: rule-set-as-subset
type: pattern
status: active
created: 2026-09-16
updated: 2026-09-16
author: heiniao
source: 契约设计 + 集合论
aliases: [rule-set-as-subset]
provenance: collab apply 门禁 —— contentRules ⊂ standardRules；刚落盘的条目还不满足索引不变量
---

# 规则集分层：子集即契约

## 上下文

同一套校验规则要在不同生命周期阶段使用（如"刚写入的文件" vs "入库后的工作区"）。

## 问题

- **两份并列清单**（`contentRules` 与 `standardRules` 各维护一列）→ 必然漂移，且看不出二者关系。
- **一律跑全量规则** → 合法的中间态被误判为错误（新条目还没进 `_index.md`）。
- **一律跑子集** → 索引悬空、死链等入库后不变量永远查不到。

## 方案

把规则集建成**基集 + 扩展**，让"谁是谁的子集"在代码里可见：

```ts
const contentRules = [/* 条目自身 */];
const standardRules = [...contentRules, /* 索引 / catalog 等关系规则 */];
// 不变量：contentRules ⊂ standardRules
```

| 阶段 | 用哪套 | 为什么 |
| :--- | :--- | :--- |
| 落盘门禁 / 单条内容 | 基集 | 索引行还不存在，跑扩展必红 |
| 全量 `validate` | 基集 ∪ 扩展 | 关系不变量此时必须成立 |

**子集关系本身是设计信息**：它把"刚落盘还不满足索引不变量"写成可执行的结构，而不是注释里的口头约定。

## 反面

- 不要维护两份互不派生的规则表。
- 不要用"特殊开关"绕过规则来表达生命周期 —— 换规则集比关检查更诚实。
- 不要把"子集"写成文档却在代码里复制粘贴 —— 漂移从第一天开始。

## 关联

[[patterns/three-level-dry]] [[patterns/allowlist-over-denylist]] [[patterns/derivation-over-copy]] [[A10-review-前置原则]]
