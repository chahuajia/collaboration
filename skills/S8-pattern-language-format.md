---
id: S8
type: skill
status: active
created: 2026-09-11
updated: 2026-09-17
domains: [meta]
applies-to: [all-entries]
supersedes: null
author: heiniao
aliases: [S8]
trigger: 写条目只有方案不知何时用；或缺上下文/反面读者看不出边界
enforced: null
---


## 上下文

条目若只写“怎么做”，无法传递适用边界和反面案例。

## 问题

读者不知道什么时候用、什么时候不用。

## 方案

每个条目采用 Alexander 模式语言格式：

1. **上下文**：什么时候会遇到？
2. **问题**：具体症状是什么？
3. **方案**：怎么做？
4. **反面**：什么时候不该用？误用会怎样？
5. **关联**：双向链接。

## 反面

- 不要省略“反面”。
- 不要只写方案不写上下文。

## 关联

[[patterns/pattern-language]] [[S9-yaml-metadata]] [[ROOT]]