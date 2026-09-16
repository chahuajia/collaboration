---
id: how-as-injected-function
type: pattern
status: active
created: 2026-09-16
updated: 2026-09-16
author: heiniao
source: 六边形架构 + 依赖倒置
aliases: [how-as-injected-function]
provenance: collab apply 的内容哈希 —— 领域既不能 import node:crypto，又不能把"算哈希"拆成两步推迟到外层
---

# How 以函数注入

## 上下文

领域层需要一个**纯计算**能力（哈希、排序键、规范化），但实现绑在运行时库上（如 `node:crypto`）。

## 问题

- **领域 import 第三方** → 破坏 [[patterns/domain-purity-is-structural]]。
- **把能力推迟成两步端口** → 调用方被迫持有半成品，API 变丑，还容易漏调。
- **在领域里重写一份算法** → 第二份真相源，必然漂移。

## 方案

领域只声明**函数类型**（What 的形状），实现由外层注入（How）：

```ts
export type ContentHasher = (content: string) => string;

// 领域：吃 hasher，不 import crypto
export function planApply(bundle: Bundle, hash: ContentHasher): Plan { ... }

// 基础设施：提供具体 How
const sha256: ContentHasher = (s) => createHash("sha256").update(s).digest("hex");
```

**判据**：

| 能力 | 做法 |
| :--- | :--- |
| **纯计算**（同输入同输出、无 IO） | 函数类型注入领域 |
| **带 IO**（读盘、读网、读时钟） | 只能走端口 / 应用层 |

这与 [[patterns/dependency-decision]] 互补：那边问"要不要引入依赖 / 放哪层"；这边问"**已经需要的能力**如何既不脏领域、又不拆坏 API"。

## 反面

- 不要把带 IO 的东西伪装成纯函数塞进领域。
- 不要在领域里写 `hash = require("crypto")` 的"临时例外"。
- 不要为每个纯函数都建完整端口接口 —— 函数类型够用时，接口是过度设计。

## 关联

[[patterns/domain-purity-is-structural]] [[patterns/dependency-decision]] [[patterns/what-how-are-projections]] [[S12-边界解析]]
