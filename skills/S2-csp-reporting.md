---
id: S2
type: skill
status: active
created: 2026-09-11
updated: 2026-09-17
domains: [security, nextjs]
applies-to: [W1]
supersedes: null
author: heiniao
aliases: [S2]
trigger: CSP 违规页面空白却无告警；或不知怎么配 report-uri
enforced: null
---

# S2 CSP 报告配置

## 上下文

生产环境 CSP 违规是静默失败，用户看到页面空白，开发者不知情。

## 问题

无法观测 CSP 拦截事件，问题定位滞后。

## 方案

1. CSP 头加 `report-uri` 和 `report-to`。
2. `Report-To` 头定义报告组。
3. 报告端点用同域 `/api/csp-report`。
4. 开发环境不发报告。
5. 用 `Content-Security-Policy-Report-Only` 做灰度。
6. 报告按 `(blocked-uri, directive, document-uri)` 聚合限流。
7. 告警分级：第三方域名 = P1，自家新接口遗漏 = P2。

## 反面

- 不要在 CSP 里硬编码 localhost。
- 不要把报告端点放在外部域名（会再次引入 CSP 问题）。
- 不要在开发环境发报告，噪音太大。

## 关联

[[W1-blank-page-triage]] [[S3-env-fail-fast]] [[patterns/layered-defense]]