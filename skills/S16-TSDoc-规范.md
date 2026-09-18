---
id: S16
type: skill
status: active
created: 2026-09-13
updated: 2026-09-17
domains:
  - typescript
  - documentation
applies-to:
  - all
supersedes:
author: heiniao
aliases:
  - S16
trigger: 公开 API 注释乱/缺失；或只写怎么做不写为什么
enforced: null
---



# S16 TSDoc 规范

## 上下文

公开导出的类型/函数需要让使用者快速理解其用途。

## 问题

- 注释缺失或格式混乱。
- 只写"怎么做"，不写"为什么"。

## 方案

- **首句摘要**：一句话说明用途。
- **`@remarks`**：为什么这样做、什么时候不该用。
- **`@typeParam` / `@param` / `@returns` / `@throws`**：签名相关。
- **`@example`**：复杂 API 必须给。
- **`@see`**：关联 API。
- **`@deprecated` / `@internal`**：状态标记。

### 必须写 TSDoc 的场景

| 场景 | 是否必须 |
| :--- | :--- |
| 公开导出的类型/函数 | ✅ |
| 领域核心概念（值对象、实体） | ✅ |
| 复杂算法 | ✅ |
| 内部实现细节 | ⚪ 可选 |
| 显而易见的 getter/setter | ❌ |

## 反面

- 不要为每个变量写注释。
- 不要用中文冒号替代 `@tag`。
- 不要只写"这个函数做什么"而不写"为什么"。

## 关联

[[S8-pattern-language-format]] [[patterns/pattern-language]]
