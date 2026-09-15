---
id: S35
type: skill
status: active
created: 2026-09-14
updated: 2026-09-14
domains: [typescript, typing]
applies-to: [S15, S21]
author: <待填>
aliases: [S35]
---

# S35 key/value 空间区分

## 上下文

用 `const object + as const` 定义枚举时，有**两个"空间"**：

- **key 空间**：如 `'Agreement' \| 'Workflow' \| 'Skill'`（PascalCase）
- **value 空间**：如 `'agreement' \| 'workflow' \| 'skill'`（lowercase）

两个空间的类型**不同** —— **不能互相赋值**。

## 问题

- **断言 `value as key`** —— `kind as EntryTypeKey` —— **运行时错**。
- **`Object.entries(obj)` 的 key** —— 是 `string` —— **丢失精度**。
- **两个不同的 const object** —— 如 `EntryKindValues`（key 是 PascalCase）和 `EntryKindDir`（key 是 lowercase）—— 混用。

## 方案

### 一、判据："它的 key 是什么类型？"

| 对象 | key 空间 | value 空间 |
| :--- | :--- | :--- |
| `EntryKindValues` | PascalCase | lowercase |
| `EntryKindDir` | lowercase | 目录名 |

**两个空间的 key 不同** —— **不能断言**。

### 二、遍历时"选对空间"

**需求**："从 path 推断 PascalCase key"（用于 `makeEntry`）。

**正确做法**：

```ts
for (const key of Object.keys(EntryKindValues)) {
  if (!isEntryKindKey(key)) continue;
  const dir = EntryKindDir[EntryKindValues[key]];
  if (normalized.startsWith(dir + '/')) {
    return key;
  }
}
```
**关键**：

- **遍历 `EntryKindValues` 的 key** —— 拿到 PascalCase。
    
- **`EntryKindValues[key]`** —— 用 key 取值（lowercase）。
    
- **`EntryKindDir[value]`** —— 用值查目录。
    
- **返回 key** —— 类型是 PascalCase。
    

### 三、类型守卫代替断言
```TS
function isEntryKindKey(value: string): value is keyof typeof EntryKindValues {
  return value in EntryKindValues;
}
```
**用 `in` 做运行时验证** —— **不用 `as`**。

### 四、`Object.entries` 的"类型黑洞"

**`Object.entries(obj)`** 返回 `[string, V][]` —— **key 丢失精度**。

**替代**：

- **`Object.keys(obj)` + 类型守卫**。
    
- **`for...in` + 类型守卫**。
    
- **显式列表**（最稳，但要手动同步）。
    

## 反面

- 不要 `value as key`。
    
- 不要假设"两个 const object 的 key 一样"。
    
- 不要用 `Object.entries` 后直接断言。
    
- 不要混用"两个空间的 key"。
    

## 关联

[[S15]] [[S21]] [[patterns/derivation-over-copy]]
