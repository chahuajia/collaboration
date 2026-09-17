# AGENTS.md

> 本文件是 **AI 进入本仓库的入口**。人类用户读 `README.md`。
> 原则：**入口指向，不复制内容**（[[A13-AI-入口文件规范]]）。本文件保持 100 行以内。

## 项目是什么

本仓库是 **COLLABORATION 知识库本体**（布局 B：**仓库根 = 知识库根**）。
它把与 AI 长期协作的经验沉淀为五类可检索条目——约定 / 工****作流 / 技能 / 模式 / 领域，
并用生命周期与修剪策略对抗文档腐烂（[[pruning-policy]]）。

## 阅读顺序

1. **本文件下面的「症状 → 条目」表** —— **手写路由，按"你要干什么"组织**。
   实测命中率高于 `catalog.json`（2/4 vs 0/4）—— 因为路由的正确组织方式是**按症状**，
   不是按条目清单。
2. **`catalog.json`** —— **生成物**（`collab catalog`），当"目录 + 补充检索"用。
   ⚠️ **它现在还不是完整路由表**：`trigger` 只覆盖 **11/108** 条，
   且那 11 条恰好是"本来就会读的"（9 条约定 + 2 条模式）。
   直接依赖它的 `trigger`，等于把 97 条当不存在。**填充 `trigger` 是待办，不是现状。**
3. `ROOT.md` —— 结构、权限、演化机制（最短总览）
4. `meta/pruning-policy.md` —— 条目怎么诞生、休眠、退役
5. `meta/base-contract.md` —— **冻结的基座**（目录 / kind / id / frontmatter / 链接 / 生成物）；
   动它 = 破坏性变更，需要 ADR + 迁移脚本 + `collab validate` 归零
6. **按需读取条目，不要全量读取**（上下文是预算，不是容器 —— [[A16-上下文预算法]]）：
   - 边界、权限、不可谈判的规则 → `agreements/`
   - 复杂任务的执行剧本 → `workflows/`
   - 可复用的具体做法 → `skills/`
   - 概念、判据、跨领域借鉴 → `patterns/`
   - 特定环境下的操作手册（对话式 AI / 有 IO 的 agent） → `integrations/`
   - 按领域找入口 → `domains/`
6. **工作记忆不在本仓库**：本仓库只放**长期知识**。当前进度、决策日志、锚点属于「工作记忆」，
   实体在 **`collab-cli/working-memory/`**（见 [[W10-working-memory]]）。
   本仓库内**没有** `working-memory/` 目录 —— 别去找，也别在这里新建。

> **路由优先级**（2026-09-16 按实测校正）：
> 1. **先看下面的症状表** —— 它是"我要做 X → 读 Y"的映射，命中最快
> 2. 症状表没覆盖 → 用 `catalog.json` 做**关键词检索**（它是目录，不是路由）
> 3. 都没有 → **报告"找不到"**，不要凭空发明规范；**并记进 [[known-gaps]]（缺口账本）**
>
> **为什么校正**：原话写"先看 catalog 的 trigger"，但实测 `trigger` 只在 11/108 可用，
> 且那 11 条不需要路由。**一个 89% 缺席的路由表，会让人以为"查过了、没有"。**

## 症状 → 条目（路由表）

> 按症状组织，不是按条目清单。表里没有 → `catalog.json` 关键词检索 →
> 仍没有 → **报告"找不到"**，记进 [[known-gaps]]；**不要发明规范**。

| 遇到的情况 | 先读 |
| :--- | :--- |
| 页面空白 / 数据不出来 | [[W1-blank-page-triage]] |
| 要设计一个新结构或抽象 | [[patterns/design-decision]] |
| AI 写了大量代码等我确认 | [[A10-review-前置原则]] |
| 问题空间不明确，写不出测试 | [[W9-Spike-工作流]] |
| 不确定要不要引入依赖 / 放哪 | [[dependency-decision]] |
| 出了故障要定位 | [[W1-blank-page-triage]] |
| 任务做完要沉淀 | [[W4-three-question-retro]] → [[W5-update-collaboration]] → [[meta/pruning-policy]] |
| 想改约定 | [[W7-rfc-process]] |
| 多条条目重叠要合并 | [[W12-条目合并]] |
| 要跨领域找灵感 | [[patterns/cross-domain-borrowing]] |
| **不变量放哪一层** / 聚合边界 | [[S13-Smart-Constructor]] → [[patterns/parse-dont-validate]]（对象自守；跨对象由聚合操作守） |
| **领域层能不能碰框架** | [[patterns/domain-purity-is-structural]] |
| **状态机怎么建模**（领域） | **明确不建专条**（2026-09-16 第 5 轮裁决）→ 复用实体状态机；证伪条件见 `known-gaps` 已关闭行 |
| **多资源 REST 读法 / 客户端 N+1** | [[patterns/design-decision]]（batch 端点 vs 循环 GET）；GraphQL 先过 [[dependency-decision]] |
| **边界错误怎么映射 HTTP**（API/CLI） | [[S34-边界层与领域的错误翻译]]（类型化异常；禁止用领域 message 前缀猜状态码） |
| **压测放哪 / 长运行空转 / 测数涨 KB 不涨** | [[patterns/pressure-routing]]（L1 工具链 vs L2 业务 vs L3 KB；每 tick 问期望哪个 HEAD 变） |

## 协作规则（摘要）

- 回答从 H2 开始（[[S1-h2-output]]）；不客套、不堆砌。
- **先给规格再写实现**；review 对象优先级：规格 > 测试 > 类型 > 实现（[[A10-review-前置原则]]）。
- 每个设计决策要能回答："不做会怎样 / 收益是什么 / 成本是什么"（[[patterns/design-decision]]）。
- 不确定就问，不猜；先看地图再点菜（[[chatgpt-paste-protocol]]）。
- 复杂讨论结束附一份**面向人的知识笔记**（[[A12-知识笔记返回]]），它与条目是两种表达，不是复制。
- **新增条目必须能说出它拦住了什么**；说不出来就别进库（[[meta/pruning-policy]]）。

## 边界

- **AI 不 commit、不 push**：改动留在工作区，由人确认（[[cli-agent-boundaries]]、[[A6-version-authority]]）。
- 约定级变更走 [[W7-rfc-process]]；技能/模式可自由增删，但要走同一套校验。
- 本文件**不存放**日常进度、任务、决策——它们属于工作记忆或条目本身。

## 关联

[[ROOT]] [[S1-h2-output]] [[A6-version-authority]] [[cli-agent-boundaries]] [[patterns/design-decision]] [[patterns/cross-domain-borrowing]] [[patterns/domain-purity-is-structural]] [[patterns/parse-dont-validate]] [[S13-Smart-Constructor]] [[A10-review-前置原则]] [[A12-知识笔记返回]] [[A13-AI-入口文件规范]] [[chatgpt-paste-protocol]] [[A16-上下文预算法]] [[W1-blank-page-triage]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[W7-rfc-process]] [[W9-Spike-工作流]] [[known-gaps]]
