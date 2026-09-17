# Next.js 领域

## 相关条目

- [[patterns/frontend-ddd-rsc]] **前端 DDD + RSC 边界**（默认读模型 RSC；客户端仅岛）
- [[S2-csp-reporting]] CSP 报告
- [[S3-env-fail-fast]] 环境变量 fail-fast
- [[S6-sw-debugging]] Service Worker 调试
- [[S7-ddd-frontend-light]] DDD 前端轻量化（历史技能；渲染策略以 frontend-ddd-rsc 为准）

## 症状 → 先读

| 症状 | 条目 |
| :--- | :--- |
| 全站 `"use client"` + useEffect 拉数 | [[patterns/frontend-ddd-rsc]] |
| `NEXT_PUBLIC_*` 编译期内联踩坑 | [[S3-env-fail-fast]] |
| CSP / SW 缓存导致白屏 | [[S2-csp-reporting]] · [[S6-sw-debugging]] · [[W1-blank-page-triage]] |

## 常见问题

- App Router 私有文件夹与路由边界
- rewrite `/api` → 后端 vs 客户端直连 CORS
- RSC 中 `fetch` 的 `cache` / `revalidate` 必须显式
