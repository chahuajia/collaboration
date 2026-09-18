---
id: shared-kernel-across-bc
type: pattern
status: active
created: 2026-09-17
updated: 2026-09-17
source: DDD（共享内核）+ 限界上下文依赖纪律
author: heiniao
aliases:
  - shared-kernel-across-bc
  - P-shared-kernel
trigger: 跨包 import 像有问题（如 mall→commerce）；或不确定跨 BC 能否共用 Money/Account/Ledger
provenance: 2026-09-17 IDE/架构疑问 mall import commerce
enforced: null
---

# 跨 BC 只依赖共享内核，不依赖对方核心聚合

## 上下文

多限界上下文（BC）并存时，IDE 与包依赖会暗示「隔壁模块能 import 就 import」。
[[patterns/layer-vs-context]] 已区分「层 ≠ 上下文」，并列出上下文映射含**共享内核**；本模式把**允许 / 禁止的依赖对象**写死。

## 问题

- 把「同仓 / 同 monorepo」当成「同一模型」→ BC A 直接依赖 BC B 的**核心聚合**。
- 共享概念（钱、账户、分录）与**业务所有权**混淆：需要 Money 时顺手 import 了对方的 Entitlement / Order。
- 反向依赖一旦形成，统一语言与发布边界一起糊掉，改一侧必伤另一侧。

## 方案

### 判据

| 依赖目标 | 跨 BC 是否允许 | 说明 |
| :--- | :--- | :--- |
| **共享内核**（稳定、无单边业务所有权） | **允许** | 如 Money、Account、Ledger、通用 ID/时间约定 |
| **另一 BC 的核心聚合 / 领域服务** | **禁止** | 如 entitlement、订单状态机、定价策略归属方 |
| **另一 BC 的应用 API / 防腐后的 DTO** | **按映射选用** | 客户-供应商 / ACL / OHS；不是直取对方 domain |

**共享内核**须双方共同演进（或明确上游冻结）；一方私有的不变量不得塞进「共享」包。

### 例子（evolutionary）

| 场景 | 判定 |
| :--- | :--- |
| **mall → commerce**：mall 依赖 commerce 侧（或抽出的）**Money / Account / Ledger** 等共享内核 | **允许** —— 钱与账本语言跨商场/交易上下文稳定共享 |
| **mall → commerce 的 Entitlement（或同类核心聚合）** | **禁止** —— 权益归属 commerce（或专门 BC）的核心模型；mall 需要结果时走应用契约 / ACL，不 import 对方聚合根 |

口诀：

> **共享的是原料与账本词汇；不共享的是对方的业务心脏。**

### 落地检查

1. import / `project` 引用落在哪一层？domain 直连对方 domain 聚合 → 红灯。  
2. 类型名是否带对方 BC 的业务动词/名词（Entitlement、SettlementPolicy…）？有 → 多半不是内核。  
3. 能否把该类型挪到 `shared-kernel`（或等价包）而不带走单边不变量？不能 → 留在归属 BC，对外只暴露契约。

与 [[patterns/domain-purity-is-structural]] 一致：破坏的是**依赖图**，不是「这次方便一下」。

## 反面

- 不要用「反正会编译」证明跨 BC domain→domain 合法。
- 不要把对方聚合「复制一份进自己 BC」冒充解耦（见 [[patterns/derivation-over-copy]]）。
- 不要把尚未双方认可的类型提前放进共享内核。

## 关联

[[patterns/layer-vs-context]] [[patterns/domain-purity-is-structural]] [[patterns/value-object-as-raw-material]] [[patterns/derivation-over-copy]] [[S7-ddd-frontend-light]]
