---
id: S7
type: skill
status: active
created: 2026-09-11
updated: 2026-09-17
domains: [architecture]
applies-to: [W3]
supersedes: null
author: heiniao
aliases: [S7]
trigger: 前端要上 DDD 怕过度设计；或不知后端战术怎么映射
enforced: null
---

# S7 DDD 前端轻量化落地

## 上下文

前端项目引入 DDD，容易照搬后端战术模式导致过度设计。

## 问题

- 聚合根、仓储接口在前端无意义。
- 领域事件在前端可用更轻的 EventEmitter。

## 方案

DDD 概念的前端映射：

| 后端 DDD | 前端对应 |
| :--- | :--- |
| 限界上下文 | Feature 模块 |
| 实体/VO | TS 类型 |
| 聚合 | Apollo 缓存 / Zustand |
| 仓储 | API 层函数 |
| 领域服务 | 纯函数 |
| 应用服务 | 自定义 Hooks |
| 领域事件 | EventEmitter / mitt |

 目录：
`features/<domain>/`  
`├── api/`  
`├── domain/`  
`├── application/`  
`├── ui/`  
`└── index.ts`

## 反面

- 不要写 `ICartRepository` 接口。
- 不要为每个实体写领域服务。
- 不要在 UI 组件里写业务规则。

## 关联

[[W3-ddd-refactor]] [[patterns/load-bearing-vs-partition]] [[domains/architecture/_index]]