---
id: S20
type: skill
status: active
created: 2026-09-13
updated: 2026-09-13
domains:
  - typescript
applies-to:
  - S13
supersedes:
author: heiniao
aliases:
  - S20
---

# S20 Result.all 组合

## 上下文

需要把多个 `Result` 组合成一个大 `Result`（全部成功 → 成功；任一失败 → 失败）。

## 问题

- 手动 `if (a.ok && b.ok && c.ok)` 冗长。
- 用 `as` 断言绕过类型窄化会破坏 Result 的封闭性。

## 方案

```ts
export function all<T, E>(results: readonly Result<T, E>[]): Result<T[], E[]> {
  const values: T[] = [];
  const errors: E[] = [];
  for (const r of results) {
    if (r.ok) values.push(r.value);
    else errors.push(r.error);
  }
  return errors.length > 0 ? Err(errors) : Ok(values);
}

````

**变长元组版本**（保留所有位置类型）需要 mapped type——见未来 S25。

## 反面

- 不要用 `throw` 处理"某些 Result 失败"（违反 Result 封闭性）。
    
- 不要在组合时用 `as { ok: true }` 断言。
    

## 关联

[[S13-Smart-Constructor]] [[S25-combineResult-变长元组]]
