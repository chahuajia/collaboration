---
id: S25
type: skill
status: draft
created: 2026-09-13
updated: 2026-09-13
domains: [typescript]
applies-to: [S20]
supersedes: null
author: <待填>
aliases: [S25]
---

# S25 combineResult 变长元组

## 上下文

`S20` 的 `all` 是简化版——所有 Result 的值类型相同。当值类型不同时（如 `Result<EntryId>`, `Result<ISODate>`），需要保留每个位置的类型。

## 问题

- `all([Ok(1), Ok('a')])` → `Result<[number | string], ...>`，丢失元组位置。
- 需要 `Result<[number, string], ...>`。

## 方案

用 mapped type 保留每个位置：

```ts
export function combineResult<T extends readonly unknown[], E>(
  results: { [K in keyof T]: Result<T[K], E> },
): Result<T, E[]> {
  // ...
}

```
**状态**：草案。等 S20 简化版在真实场景下不够用时，再落地。

## 反面

- 不要为了"类型完备"而牺牲"易用"。
    
- 简化版够用时，不引入变长元组版本。
    

## 关联

[[S20]]
