# COLLABORATION

> 与 AI 协作的元规范。**它同时是一个自我演化的知识实体**：
> 条目会诞生、被引用、休眠、退役——不维护它，它就腐烂。

## 三句话入门

- **约定**（`agreements/`）：必须遵守的协作规则，双方确认才能改。
- **工作流**（`workflows/`）：复杂任务的执行剧本，可复盘迭代。
- **技能 / 模式**（`skills/` `patterns/`）：可复用的做法与判据，可自由增删。

## 目录

| 路径 | 内容 |
| :--- | :--- |
| `AGENTS.md` | **AI 的入口**（薄入口，只指路） |
| `ROOT.md` | 结构、权限、演化机制 |
| `CORE.md` | **技术内核**：把全库压成一页（机制 / 判据 / 不变式 / 继承蓝图） |
| `SHARE.md` | 对外分享稿（叙事 + 判据 + 代价） |
| `agreements/` | 约定（协作规则，有上限） |
| `workflows/` | 复杂任务的执行剧本 |
| `skills/` | 可复用的能力单元 |
| `patterns/` | 跨领域判据 |
| `integrations/` | 特定环境的操作手册 |
| `domains/` | 领域索引 |
| `rfcs/` | 约定级变更的提案（走 [[W7-rfc-process]]） |
| `profiles/` | 个人 / 项目的侧重声明（`focus` / `exclude`） |
| `meta/` | 演化日志、ADR、命名规范、修剪策略 |
| `templates/` | 所有模板（一处一份） |
| `inbox/` | 未归档增量包（**必须长期为空**） |

> **各目录的条数不在这里写。** 手写的可计算量必然腐烂 —— 本表曾写
> "约定 16 · 工作流 10 · 模式 40"，而实际是 10 / 11 / 52；
> `meta/interceptions.md` 的表头写 7 而行有 8。**数字是命令的输出，不是文档的一部分：**
>
> ```sh
> collab catalog            # 生成 catalog.json，条目与计数都在里面
> collab validate           # 条目总数与一致性
> ```

## 如何使用

**人**：`ROOT.md` → 按需查 `_index.md` → 读条目。
**AI**：先读 `AGENTS.md`（自动加载），再按它的路由表按需取 2-3 条，**不要全量读**。

- 需求清晰、要 AI 落地 → [[A10-review-前置原则]]（review 对象优先级 + 五阶段流程 + 五对策）
- 需求不清、写不出测试 → 先 [[W9-Spike-工作流]]
- 任务结束要沉淀 → [[W4-three-question-retro]] → [[W5-update-collaboration]]

## 如何贡献

1. 识别内容属于约定 / 工作流 / 技能 / 模式（分不清就先放 `inbox/`）。
2. 从 `templates/` 复制对应模板（不要凭记忆写 frontmatter）。
3. 填 **`provenance`**：这条内容来自哪次真实事故/需求？写不出来 → 它不该存在。
4. 更新对应目录的 `_index.md`。
5. 追加 `meta/evolution-log.md`（删除和合并**也要记**）。

## 演化机制

> ⚠️ **带「未实现」的是政策，不是机制。** 逐条现状（含证据）见 [[pruning-policy]] 的
> 「机制成熟度」表 —— 判据是 [[patterns/policy-without-mechanism]]。

- **生命周期**：draft → active → dormant → deprecated。
- **修剪**：标记-清除**已接线**（`collab retire --candidates`）；分代、冷热分级 **未实现**。
- **代谢配额**：新增 3 条 → 处理 1 条 —— **未实现**（代码里只有「约定数上限 10」，不是 3:1 配额）。
- **结构性决策**：写入 `meta/decision-records/`。
- **约定级变更**：走 [[W7-rfc-process]]。

## 公开仓库

本规范开源，欢迎 fork 与 PR。

- 许可证：CC BY-SA 4.0（全文见 `LICENSE`）
- 主干：https://github.com/chahuajia/collaboration
- CI：`.github/workflows/validate.yml`（真 `collab validate` + KB 文档↔`--help` 检查）；
  **建议设成 required check**（GitHub：`Settings → Branches`，check 名 **`collab validate`**）——
  2026-09-26 实测：这个 workflow 从 #1 到 #11 连续红着，而没人被通知到
- 贡献流程：[[W6-local-patch-to-community-pr]]（本地补丁 → 你的 fork → 社区 PR）；约定级变更走 [[W7-rfc-process]]
- AI 写权限边界：[[cli-agent-boundaries]]
- 个人侧重：[[S11-profile-declaration]] · `profiles/`
- 快速开始：fork → 复制 `templates/profile-template.yaml` 为 `profiles/<你的用户名>.yaml` → 声明 `focus` / `exclude` → 用 `collab` CLI（[[S10-collab-cli]]）
