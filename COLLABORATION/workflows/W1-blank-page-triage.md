---
id: W1
type: workflow
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [react, nextjs, graphql, security]
applies-to: [blank-page, data-missing]
supersedes: null
author: heiniao
---

# W1 页面空白/数据不出来的黄金排查路径

## 上下文

前端页面空白或数据不出来，原因可能在前端、后端、配置、缓存、安全策略任一层。

## 问题

排查顺序错误会导致反复试错，浪费时间。

## 方案

按以下顺序排查，前一步排除后再进入下一步：

1. **Console**：有无 CSP 违规、TypeError、GraphQL 错误。
2. **Network**：有无请求？
   - 无请求 = CSP/CORS/代码未执行。
   - 有请求但 4xx/5xx = 后端问题。
3. **请求参数**：用相同参数直接调 API（curl/Postman）复现。
4. **响应数据**：对比 API 返回与 UI 期望。
5. **Service Worker**：是否缓存旧响应？Application → SW → Unregister。
6. **环境变量**：`NEXT_PUBLIC_*` 是否编译期内联？改后需重启 dev server。

## 反面

- 不要跳过 Console 直接看 Network。
- 不要忽略 Service Worker 缓存。
- 不要在未验证参数时断定后端有问题。

## 关联

[[S2]] [[S3]] [[S4]] [[S6]] [[patterns/layered-defense]]