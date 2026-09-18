---
id: S13
type: skill
status: active
created: 2026-09-13
updated: 2026-09-13
domains:
  - ddd
  - typescript
applies-to:
  - W3
supersedes:
author: heiniao
aliases:
  - S13
trigger: 不变量放哪一层；或差点把可用性守在构造器上
enforced: evolutionary:backend/src/test/java/com/evolutionary/station/domain/StationTest.java
---

# S13 Smart Constructor

## 上下文

需要构造一个有不变量的领域对象（值对象、实体）。

## 问题

- 直接 `new` 或字面量构造会绕过校验。
- 不变量分散在多个地方。

## 方案

- 构造函数私有（`private constructor`）。
- 提供静态工厂 `create`（可能失败）或 `of`（信任输入）。
- `create` 返回 `Result<T, Issue>`。
- **每个值对象独立承担自己的不变量**。
- 跨值对象的不变量由上层聚合的 `create` 承担。

## 反面

- 不要暴露 `constructor`。
- 不要用 `as` 断言绕过构造。
- 不要在 `create` 里塞太多不相关的校验。

## 关联

[[S12-边界解析]] [[S24-create-vs-of]] [[patterns/value-object-as-raw-material]]
