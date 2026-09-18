---
id: S9
type: skill
status: active
created: 2026-09-11
updated: 2026-09-17
domains: [meta]
applies-to: [all-entries]
supersedes: null
author: heiniao
aliases: [S9]
trigger: 条目多了只能按路径翻；或要按域/状态横切检索
enforced: null
---

# S9 YAML 元数据

## 上下文

条目增多后，需要多维检索。

## 问题

纯目录结构只能按路径查找，无法横切。

## 方案

每个条目头部加 YAML：

```yaml
---
id: S5
type: skill
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [api, react, database]
applies-to: [W1]
supersedes: null
---
```
字段含义：

- `status`: draft / active / dormant / deprecated
    
- `domains`: 跨领域标签
    
- `applies-to`: 被哪些工作流调用
    
- `supersedes`: 替代了哪个条目
## 反面

- 不要省略 `status`。
    
- 不要滥用 `domains`，只标真正相关的。
## 关联

[[S8-pattern-language-format]] [[patterns/faceted-classification]] [[meta/pruning-policy]]