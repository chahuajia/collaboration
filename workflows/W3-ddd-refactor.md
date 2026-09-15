---
id: W3
type: workflow
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [architecture]
applies-to: [directory-bloat, module-coupling]
supersedes: null
author: heiniao
aliases: [W3]
---

# W3 DDD 架构重构工作流

## 上下文

多应用、多模块、多状态项目，目录扁平化膨胀，状态互相纠缠。

## 问题

- 技术分层与业务边界脱节。
- 改一处而动全身。
- 状态管理混乱。

## 方案

1. **划清限界上下文**：识别业务域（cart/ordering/user/location）。
2. **建立通用语言**：消灭同物异名，类型定义对齐。
3. **设计防腐层**：API 数据结构不直接污染 UI。
4. **前端轻量化落地**：
   - Hooks 代替应用服务
   - Apollo 缓存代替仓储
   - 纯函数代替领域服务
5. **绞杀者模式迁移**：从最痛的域开始，逐步替换。

目录结构：
- `app/` 路由层
- `features/` 业务域
- `components/` 全局共享 UI
- `lib/` 基础设施

## 反面

- 不要照搬后端 DDD 战术模式（聚合根、仓储接口）。
- 不要一次性重构。
- 不要为无业务的页面强加领域模型。

## 关联

[[S7]] [[patterns/load-bearing-vs-partition]] [[domains/architecture/_index]]