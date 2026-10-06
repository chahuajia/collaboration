---
id: one-kb-per-org
type: pattern
status: active
created: 2026-10-06
updated: 2026-10-06
author: heiniao
aliases:
  - one-kb-per-org
  - 唯一知识库
  - 第二本库
trigger: 业务仓里长出了 agreements/ meta/ patterns/ catalog.json；或发现两本同名账本（两个 known-gaps / 两个 interceptions）/ 两个 catalog.json；或有人问"每个项目要不要各建一个知识库"
provenance: 2026-10-06 实测 —— food-delivery 项目接入了全局 KB，却同时手建了一套 agreements/ + meta/（含第二本 known-gaps）+ patterns/ + catalog.json：本地 catalog 3 条、共享库 132 条，而入口路由指向本地那本，agent 因此整段会话都没读到共享库。本条目与同批的"迁 / 并 / 删"修复一起长出来
falsifier: 会照着"这个项目需要自己的规范，那就照知识库的目录结构建一份"的本地直觉继续写 —— 结果是同一事实有两个家：两本 known-gaps、两个 catalog、两套索引，改哪本都不算改完
enforced: null
---

# 一个组织只有一个知识库

## 上下文

协作知识库的价值**全部来自累积**：拦截攒起来才知道哪条承重，缺口攒起来才知道该补什么。
而 `collab init --profile kb` 造的是**无谱系的孤儿库**；要和别人的库发生交换，用的是 **fork**（继承 + 有谱系）。
所以"项目也需要规范"这件事有正确解，但**都不需要第二个库**。

## 问题

实测（2026-10-06）：一个业务仓接入全局 KB 之后，**同时**长出了自己的一套：

| 本地东西 | 与共享库的关系 | 后果 |
| :--- | :--- | :--- |
| `catalog.json`（3 条） | 共享库 `catalog.json`（132 条） | 两个目录；本地那个是**陈旧生成物**，读它等于读一份过期索引 |
| `meta/known-gaps.md` | KB 也有一本 `known-gaps` | **两本同名账本**；查缺口读到哪本，取决于入口指向谁 |
| `meta/interceptions.md` | KB 也有一本 `interceptions` | 项目侧那本是**空的** → 收益侧永远不累积，修剪失去依据 |
| `meta/pruning-policy.md` | 是 KB 那份的**副本** | 政策有两份，改一边忘另一边 |
| `agreements/` `patterns/` | KB 的条目目录 | 通用判据被写进业务仓：换项目就是噪音，也不会被别人读到 |

最贵的一条**不是内容错，而是入口指错**：项目入口路由到本地的 `catalog.json` 与 `meta/known-gaps.md`，
agent 于是整段会话都没读到共享库 —— 知识库一次都没增长。

## 方案

### 一、判据：这个仓里是不是已经有一本"第二本库"

出现下列任一，它就不是"项目文档"，而是**第二本库**：

- `catalog.json`（生成物；一个组织只需要一个）
- `meta/known-gaps.md` / `meta/interceptions.md` / `meta/pruning-policy.md`
- `agreements/` `patterns/` `workflows/` `skills/` 下的**条目**（带条目 frontmatter 的 `.md`）
- 各目录的 `_index.md`（索引同样是生成物）

### 二、三种正确落点（先分类，再动手）

| 内容 | 去哪 | 判据 |
| :--- | :--- | :--- |
| **通用判据／可复用做法** | **KB**（经 CLI 写入 + 校验 + 编目） | 换个项目还成立吗？成立 → KB |
| **项目证据**（进度、待裁决、项目特有缺口、轮次报告） | 项目 `working-memory/` | 只有这个仓读得懂 → WM（见 [[patterns/project-evidence-vs-kb-ledger]]） |
| **项目文档**（架构地图、迁移方案、验证策略） | 项目 `docs/` | 它**不是库**：没有条目 frontmatter、不进 catalog、不做索引 |

### 三、修法：迁 / 并 / 删（不许"两边都留一份以防万一"）

1. **迁**：本地条目逐条按上表判归属；进 KB 的必须**走 CLI**（`new`，或 `parse` → `apply`），不要手写。
2. **并**：同名账本只留一本 —— KB 侧留**一行摘要 + 证据链接**，证据正文留在项目 WM。
3. **删**：本地 `catalog.json`、`_index.md`、KB 政策副本一律删（要么是生成物，要么是复印件）。
4. **改入口**：项目 `AGENTS.md` 只说"长期知识在哪个 KB" + 一条命令，**不复制条目内容**
   （[[A13-AI-入口文件规范]]：入口指向，不复制）。
5. **验收**：`collab doctor` 查"入口有没有点名工具 / 生成物新不新 / 校验过不过 / 有没有 `.git`"；
   有 ❌ 就是没修完。

### 四、想要自己的库：fork，不是 init

| 想要 | 做法 |
| :--- | :--- |
| 只是"项目也要用规范" | **指向**已有的 KB（consumer profile）—— 不需要第二个库 |
| 想要一套带自己特性的库 | **fork** KB 仓（有谱系，能选择性回灌主干） |
| 真的要从零起步 | 才 `init --profile kb` —— 但那要明说代价：**放弃与主干的杂交** |

## 反面

- 不要为了"项目自洽"把 KB 的目录结构复制进业务仓。
- 不要把项目 `docs/` 当第二本库用：它一旦开始承载通用判据，就开始腐烂。
- 不要把项目特有缺口写成 KB 缺口：那是把噪音推给所有项目。
- 不要"两边都留一份以防万一"—— 两份必然漂移，而你会以为它们是同一份。
- 不要用本地 `catalog.json` 判断"知识库有没有在用"：它是生成物，陈旧只说明**没人跑 CLI**。

## 关联

[[patterns/project-evidence-vs-kb-ledger]] [[patterns/derivation-over-copy]] [[A13-AI-入口文件规范]] [[W10-working-memory]] [[usage-guide]] [[patterns/three-layer-memory]] [[patterns/policy-without-mechanism]] [[meta/known-gaps]]
