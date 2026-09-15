# inbox —— 未归档增量

> **这个目录必须长期是空的。** 它的存在不是问题，"长期不空"才是问题。

## 它是什么

`inbox/` 存放 **AI 产出的增量包（bundle）**：AI 在一次协作里写出一批条目，
但还没有落盘成正式条目。它是**补丁**，不是**真相源**。

## 为什么需要它

协作规范是逐条演化的，但 AI 的输出是成批的。中间需要一个暂存区，
否则整批内容会以"一篇大文档"的形式沉淀下来——**不可寻址、不可链接、不可修剪**。

## 规则

| 规则 | 说明 |
| :--- | :--- |
| **不是真相源** | 与正式条目冲突时，以正式条目为准 |
| **必须清空** | 每次会话/每批落盘结束，`inbox/` 回到空状态 |
| **可追溯** | 落盘后原 bundle 可删除（git 历史保留），或移入 `inbox/archive/` |

## 当前内容

| 文件 | 状态 | 说明 |
| :--- | :--- | :--- |
| `v4.1-bundle.md` | **部分待归档** | 已落盘 58 条：A11-A16 / W8-W10 / S12-S35 / 25 个模式 / ADR-0003-0004 / 演化日志格式。**未落盘**：A17、A18、A19（对话体，A18 有 3 个冲突版本，需要人工裁决） |

### 引用了但从未写出的条目（需要裁决）

| 被引用处   | 缺失条目                                            | 建议                                         |
| :----- | :---------------------------------------------- | :----------------------------------------- |
| A16（原） | `W11` / `W12`                                   | 从未写出，已从 A16 移除引用。要么补写，要么永久删除               |
| W9（原）  | `patterns/spike-as-scaffolding`                 | 已改为指向 `patterns/design-decision`。要保留概念就补写  |
| A12（原） | `patterns/dual-expression-for-human-and-system` | 已改为指向 [[A14-多实体协作原则]]。这条概念（条目给系统、笔记给人）值得补写 |

### 落盘时发现的源缺陷（已修）

- `S22` 丢失 frontmatter 起始 `---`
- `A13` 示例代码块未闭合围栏
- `S5` 的反面/关联误复制自 `S4`；`S6` 的 `id` 误写为 `S4`
- 8 个模式（cross-domain-borrowing、dependency-decision、naming-as-definition、qian-systems-engineering、structure-over-algorithm、three-level-dry、type-as-design、value-object-as-raw-material）**原文就没有「问题」章节**，需要补一句

## 落盘工具

`scripts/extract-bundle.mjs` 把 bundle 里的 `**未来文件路径**` 段落切出来，
干跑输出清单、`--write` 输出到 `.landing/` 供人 review。落盘后本脚本可删。

## 关联

[[A4-proactive-update]] [[W5-update-collaboration]] [[W6-local-patch-to-community-pr]] [[S10-collab-cli]]
