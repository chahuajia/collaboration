---
id: ROOT
type: root
version: v3
status: active
created: 2026-09-11
updated: 2026-09-11
---

# 根节点

## 上下文

我与 AI 长期协作，需要一套可持续演化的元规范，把隐性经验显性化，让每次协作都比上次更好。

## 问题

- 协作规范散落，无法复用。
- 约定/工作流/技能混在一起，粒度不清。
- 无生命周期管理，文档必然腐烂。

## 方案

三层结构 + 有根图 + 模式语言 + 演化机制。

### 三层结构

| 层级 | 目录 | 作用 | 变更权限 |
| :--- | :--- | :--- | :--- |
| L0 约定 | `agreements/` | 必须遵守的协作规则 | 双方确认 |
| L1 工作流 | `workflows/` | 复杂任务的执行剧本 | 复盘迭代 |
| L2 技能 | `skills/` | 可复用的能力单元 | 自由增删 |

### 有根图

- 树是主结构，但允许横切标签与双向链接。
- 每个条目头部 YAML 元数据支持多维检索。
- 条目用 `[[ID]]` 互链，形成图。

### 模式语言

每个条目采用 Alexander 模式语言格式：
上下文 → 问题 → 方案 → 反面 → 关联。

### 演化机制

- 生命周期：`draft → active → dormant → deprecated`。
- 修剪策略：见 `meta/pruning-policy.md`。
- 结构性决策：写入 `meta/decision-records/`。
### 社区分发

- 分层：本地 / fork / 主干。
- 身份：Git + YAML + Profile。
- 门槛：约定 RFC，工作流 PR，技能 CI。
- AI 边界：不持主干写权限，不自动 push。
- 详见 [[A7]] [[W6]] [[W7]]。

## 反面

- 不要预设计所有分支，让结构从内容中涌现。
- 不要一开始就上所有跨领域模式，只保留真正被引用的。
- 不要让文档超过 30 秒查找成本。

## 关联

[[A1]] [[A2]] [[A3]] [[A4]] [[A5]] [[A6]] [[W4]] [[W5]] [[patterns/rooted-graph]] [[patterns/pattern-language]] [[patterns/evolution-loop]] [[A4-proactive-update]]