---
id: S5
type: skill
status: active
created: 2026-09-11
updated: 2026-09-11
domains:
  - api
  - react
  - database
applies-to:
  - W1
supersedes:
author: heiniao
aliases: [S5]
---

# S5 哨兵值替换为 null

## 上下文

用特殊值表达“无值”，如 `(0,0)` 坐标、`-1` ID、`1970-01-01` 日期。

## 问题

哨兵值是合法值，类型系统无法区分“无值”和“真实值”。

## 方案

- 用 `null` / `undefined` 表达“无值”。
- TypeScript 类型：`T | null`。
- API 契约：nullable 字段。
- 后端 Repository：入口校验哨兵值，做分支处理。

示例：
```js
const isValidCoordinate = (lat, lng) => {
  if (!Number.isFinite(lat) || !Number.isFinite(lng)) return false;
  if (lat === 0 && lng === 0) return false;
  if (lat < -90 || lat > 90 || lng < -180 || lng > 180) return false;
  return true;
};
```
## 反面
- 不要在渲染期间无条件 setState。
- 不要用 useEffect 处理可推导值。
## 关联
[[W1-blank-page-triage]] [[S5-null-not-sentinel]] [[domains/react/_index]]