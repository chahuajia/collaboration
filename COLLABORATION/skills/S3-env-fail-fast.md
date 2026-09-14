---
id: S3
type: skill
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [config, nextjs]
applies-to: [W1]
supersedes: null
author: heiniao
---

# S3 环境变量 fail-fast 校验

## 上下文

`NEXT_PUBLIC_*` 为空时静默降级，导致请求打到错误地址。

## 问题

配置错误在运行时才暴露，排查成本高。

## 方案

1. 用 Zod / envalid 定义 schema。
2. 启动或构建时校验必需变量。
3. 缺失则抛出清晰错误，附修复提示。
4. Next.js 加 `predev` 脚本检查 `.env.local`。
5. 每个仓库必须有 `.env.example`。

## 反面

- 不要用 `process.env.X || 'default'` 静默兜底。
- 不要在代码里散落 env 读取，集中到 `config/env.ts`。

## 关联

[[W1]] [[S2]] [[domains/nextjs/_index]]