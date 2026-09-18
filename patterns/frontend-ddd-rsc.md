---
id: frontend-ddd-rsc
type: pattern
status: active
created: 2026-09-17
updated: 2026-09-17
author: heiniao
source: 人机协作实践（Next 壳偏移）
aliases:
  - frontend-ddd-rsc
  - P-fe-ddd
trigger: Next 页面全 use client；或前端也要 DDD；或服务端状态与本地状态搅在一起；或缺请求缓存/弱网策略
provenance: evolutionary 用了 Next 15 却全 CSR useEffect；用户指出缺 RSC/服务端状态/请求层/前端 DDD
enforced: null
---

# 前端 DDD 与 RSC 渲染边界

## 上下文

选型 Next.js App Router 后，仍可能把所有页面写成 `"use client"` + `useEffect(fetch)`。
框架能力在，架构不在——压测壳阶段尤其容易漂移。

## 问题

- 读模型走客户端拉取 → 无 SSR/RSC、首屏空、SEO/弱网差。
- 服务真相与 UI 瞬态混在同一 `useState`。
- 裸 `fetch`：无超时、重试、缓存、失效语义。
- 「前端也是 DDD」变成只拷贝后端类型名，无 BC / 用例 / 网关分层。

## 方案

### 分层（前端 BC）

```text
app/                         # 路由与 RSC 组合根（无业务规则）
domains/<bc>/
  application/               # 用例：编排 gateway，不写 fetch 细节
  domain/                    # 只读展示模型 / 展示不变量（非后端聚合拷贝）
  infrastructure/            # HTTP gateway、DTO→模型
shared/http/                 # 统一超时、重试、错误翻译
shared/ui/                   # 哑组件
```

### 渲染默认

| 数据 | 策略 |
| :--- | :--- |
| 读多（档案、列表） | **RSC** + `fetch`（`cache` / `revalidate` 显式） |
| 静态壳 | SSG |
| 按钮/表单/乐观 UI | **客户端岛**（最小 `"use client"`） |

### 状态分工

| 层 | 职责 |
| :--- | :--- |
| 服务端（RSC + Next fetch cache） | 服务真相读模型 |
| 客户端 | 仅 UI 瞬态 |
| 禁止 | 用客户端 store 镜像整份服务端列表当源 |

### 与后端 DDD 的关系

前端 domain **不是**后端聚合的 TypeScript 移植；是**视图边界**的模型。跨 BC 共享的只有 IDL/DTO 契约字段名。

## 反面

- 不要「全站 use client」只为省事。
- 不要为上 Redux/Zustand 而镜像服务端列表。
- 不要在 page.tsx 里堆业务规则。

## 关联

[[S7-ddd-frontend-light]] [[patterns/pressure-routing]] [[patterns/parse-dont-validate]] [[S36]] [[A10-review-前置原则]]
