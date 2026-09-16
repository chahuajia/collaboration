---
id: S17
type: skill
status: active
created: 2026-09-13
updated: 2026-09-13
domains:
  - typescript
  - tooling
applies-to:
  - all
supersedes:
author: heiniao
aliases:
  - S17
---

# S17 ESLint 工具约束

## 上下文

口头约定会被遗忘，规则不会。

## 问题

- 分层依赖靠自觉。
- 类型导入风格不统一。
- 魔法数字/字符串散落。

## 方案

### 关键规则清单

| 规则 | 作用 |
| :--- | :--- |
| `@typescript-eslint/consistent-type-imports` | 强制 `import type` |
| `@typescript-eslint/no-import-type-side-effects` | 防止 `import type` 被误用 |
| `@typescript-eslint/consistent-type-assertions` | 禁止 `as` 断言（例外：`brand.ts`） |
| `@typescript-eslint/no-explicit-any` | 禁止 `any` |
| `@typescript-eslint/no-floating-promises` | 防止未 await |
| `no-magic-numbers` | 禁止魔法数字（先 warn） |
| `import/no-restricted-paths` | 分层约束（白名单） |
| `import/no-extraneous-dependencies` | 禁止 `shared/` import 外部库 |

### 分层约束（白名单）

**domain** 只允许 import `domain/` 和 `shared/`。  
**shared** 只允许 import `shared/`。

## 反面

- 不要用 `// eslint-disable-next-line` 绕过真问题。
- 不要用黑名单（穷举禁止项），用白名单（穷举允许项）。
- 不要一次上所有规则——先 warn 再 error。

## 关联

[[patterns/design-decision]] [[S12-边界解析]] [[patterns/allowlist-over-denylist]]
