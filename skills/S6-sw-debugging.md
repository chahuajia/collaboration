---
id: S6
type: skill
status: active
created: 2026-09-11
updated: 2026-09-17
domains:
  - nextjs
applies-to:
  - W1
supersedes:
author: heiniao
aliases:
  - S6
trigger: 改代码页面不变怀疑 SW；或不清怎么关掉缓存
---

# S6 Service Worker 调试

## 上下文

PWA 缓存旧响应，调试时看到过期数据。

## 问题

改代码后页面不变，误导排查方向。

## 方案

- 开发环境禁用 SW：`if (process.env.NODE_ENV === 'development') return;`
- 调试时：F12 → Application → Service Workers → Unregister。
- 或 Clear site data。
- 生产环境 SW 更新：`skipWaiting()` + 客户端提示。

## 反面

- 不要在开发环境启用 SW。
- 不要忽略缓存问题直接怀疑代码。

## 关联

[[W1-blank-page-triage]] [[domains/nextjs/_index]]

