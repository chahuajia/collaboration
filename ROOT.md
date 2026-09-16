---
id: ROOT
type: root
version: v4.1
status: active
created: 2026-09-11
updated: 2026-09-15
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

横切（不占层级，按需引用）：

- `patterns/` —— 跨领域的思想与判据
- `domains/` —— 按技术领域找入口
- `meta/` —— 演化机制本身（日志 / ADR / 命名 / 修剪）

### 有根图

- 树是主结构，但允许横切标签与双向链接。
- 每个条目头部 YAML 元数据支持多维检索。
- 条目用**双链语法**（双方括号包裹条目 ID）互链，形成图。

### 模式语言

每个条目采用 Alexander 模式语言格式：
上下文 → 问题 → 方案 → 反面 → 关联。

### 演化机制

- 生命周期：`draft → active → dormant → deprecated`。
- 修剪策略：见 `meta/pruning-policy.md`。
- 结构性决策：写入 `meta/decision-records/`。

### 选择压力（关键）

**只靠"会演化"不会变好——演化只保证适应环境。**

- 变异（新增条目）的成本已经趋近于零，**价值全部迁移到"选择"与"验证"**。
- 因此本库有三条硬机制：
  1. **可测量收益**：每次某条目真实拦住了一个错误，记入 `meta/interceptions.md`（价值 KPI 不是条目数）。
  2. **可测量成本**：引用计数与索引占用决定它是不是孤岛（见修剪策略的标记-清除）。
  3. **低成本死亡**：删除 = 退出索引，git 保留基因；不因"可惜"而保留占位。
- **代谢配额**：新增 3 条 → 处理 1 条。
### 社区分发

- 分层：本地 / fork / 主干。
- 身份：Git + YAML + Profile。
- 门槛：约定 RFC，工作流 PR，技能 CI。
- AI 边界：不持主干写权限，不自动 push。
- 详见 [[cli-agent-boundaries]] [[W6-local-patch-to-community-pr]] [[W7-rfc-process]]。

## 反面

- 不要预设计所有分支，让结构从内容中涌现。
- 不要一开始就上所有跨领域模式，只保留真正被引用的。
- 不要让文档超过 30 秒查找成本。

## 关联

[[S1-h2-output]] [[W2-three-stage-analysis]] [[A3-mutual-critique]] [[A4-proactive-update]] [[A6-version-authority]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[patterns/rooted-graph]] [[patterns/pattern-language]] [[patterns/evolution-loop]]
