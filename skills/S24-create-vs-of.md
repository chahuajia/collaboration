---
id: S24
type: skill
status: active
created: 2026-09-13
updated: 2026-09-17
domains:
  - ddd
  - typescript
applies-to:
  - S13
supersedes:
author: heiniao
aliases:
  - S24
trigger: create/of 命名混乱；或不知工厂会不会失败
---

# S24 create vs of

## 上下文

值对象和实体的工厂方法命名不统一，读者无法判断"是否会失败"。

## 问题

- 有的叫 `of`，有的叫 `create`，语义混乱。
- 调用方不知道要不要处理失败。

## 方案

| 方法 | 使用场景 | 是否失败 |
| :--- | :--- | :--- |
| **`create`** | 从**原始输入**构造，需校验不变量 | ✅ 返回 `Result` |
| **`of`** | 从**已知合法**的值构造 | ❌ 直接返回 |
| **`from`** | 从**另一种类型**转换 | 视情况 |

### 值对象 vs 实体

| 类型 | 主构造 |
| :--- | :--- |
| 值对象 | `create`（可能失败） |
| 实体 | `create` + `rehydrate`（从持久层恢复，信任数据） |

## 反面

- 不要让 `of` 出现在公共 API（它假设输入已合法）。
- 不要用 `create` 命名不失败的构造。

## 关联

[[S13-Smart-Constructor]] [[S24-create-vs-of]]
