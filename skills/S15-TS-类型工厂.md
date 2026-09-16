---
id: S15
type: skill
status: active
created: 2026-09-13
updated: 2026-09-13
domains:
  - typescript
applies-to:
  - W3
supersedes:
author: heiniao
aliases:
  - S15
---

# S15 TS 类型工厂

## 上下文

需要从 const object 派生出联合类型，或把值域映射到类型。

## 问题

- 重复手写联合类型会漂移。
- `typeof X[keyof typeof X]` 每次写太啰嗦。

## 方案

```ts
// shared/types.ts
export type ValueOf<T> = T[keyof T];
export type NonEmptyArray<T> = readonly [T, ...T[]];
export type Brand<T, B extends string> = T & { readonly __brand: B };

// shared/zod-helpers.ts
export function enumOf<T extends Record<string, string>>(
  obj: T,
): z.ZodEnum<[ValueOf<T>, ...ValueOf<T>[]]> {
  const values = Object.values(obj);
  const [first, ...rest] = values;
  if (first === undefined) throw new Error('enumOf: empty object');
  return z.enum([first, ...rest]);
}
```

## 反面

- 不要用 TS `enum` 关键字（编译产物重、不 tree-shake）。
    
- 不要让 zod schema 和 const object 各维护一份值。
    
- 不要手写 `as [T, ...T[]]`（用"首件检验"模式）。
    

## 关联

[[S12-边界解析]] [[S21-Branded-Type]]
