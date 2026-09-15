---
id: allowlist-over-denylist
type: pattern
status: active
source: 安全工程
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [allowlist-over-denylist]
---

# 白名单优于黑名单

## 上下文

需要限制某层的依赖范围。

## 问题

- 黑名单需要穷举所有禁止项，维护成本高且容易遗漏。
- 新增依赖时容易忘记加规则。

## 方案

- 声明**允许什么**，而非**禁止什么**。
- 例子（ESLint 分层约束）：

```js
{
  target: './src/domain',
  from: './src',
  except: ['./domain', './shared'],   // ← 白名单：只允许 domain 和 shared
}
```

## 反面

- 不要用"禁止 domain import zod/lodash/axios..."的黑名单。
    
- 不要假设"没想到的依赖就不用禁止"。
    

## 关联

[[S17-ESLint-工具约束]] [[patterns/dependency-decision]]
