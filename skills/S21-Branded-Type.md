---
id: S21
type: skill
status: active
created: 2026-09-13
updated: 2026-09-17
domains:
  - typescript
applies-to:
  - S13
supersedes:
author: heiniao
aliases:
  - S21
trigger: 两个 string 语义不同却能互赋；或要 branded 区分值对象
enforced: null
---

# S21 Branded Type

## 上下文

TS 是结构化类型系统，`string` 无法区分 `EntryId` 和 `EmailAddress`。

## 问题

- 两个语义不同的 `string` 互相赋值不报错。
- 值对象无法在类型层面与裸类型区分。

## 方案

```ts
// shared/brands.ts
declare const ISODateBrand: unique symbol;
export type ISODate = string & { readonly [ISODateBrand]: true };

```

- `declare const ... : unique symbol`：编译期存在，运行时不存在。
    
- **断言集中在 `brand.ts`**，其他文件 ESLint 禁止 `as`。
    
- 值对象工厂内部是唯一允许 `as` 的地方。
    

## 反面

- 不要散落 `as` 断言。
    
- 不要为每个 `string` 都造 branded type——只给有语义的。
    
- 不要忘记在 ESLint 里配 `consistent-type-assertions`。
    

## 关联

[[S13-Smart-Constructor]] [[S17-ESLint-工具约束]]
