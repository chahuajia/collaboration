---
id: layered-defense
type: pattern
status: active
source: 军事纵深防御
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [layered-defense]
---

# 分层防御

## 上下文

单点防护容易失效，需要多层拦截。

## 问题

如果只在一个层级检查，问题会漏过。

## 方案

按时间顺序分层拦截：

| 层级 | 手段 |
| :--- | :--- |
| 编译期 | 类型检查、env schema |
| 启动期 | predev 脚本 |
| 请求前 | CSP、CORS |
| 请求中 | 日志、request-id |
| 业务逻辑 | 输入校验、哨兵值处理 |
| 数据层 | Repository 单元测试 |
| 运维 | report-uri、Sentry、SW 策略 |

## 反面

- 不要只依赖一层。
- 不要在不同层重复同样的检查。

## 关联

[[W1-blank-page-triage]] [[S2-csp-reporting]] [[S3-env-fail-fast]] [[S5-null-not-sentinel]] [[ROOT]]