---
id: ADR-0011
type: adr
status: proposed
created: 2026-09-18
updated: 2026-09-18
author: heiniao
aliases: [ADR-0011]
supersedes: null
enforced: null
provenance: 修 README 手写数字时发现 interceptions 表头写 7、表里 8 行 —— 第四个被同一类缺陷骗到的人
---

# ADR-0011 新增 `enforced`：让条目能"毕业"

## 背景

本库有一个**缺失的代谢出口**。

`meta/pruning-policy.md` 规定了生命周期 `draft → active → dormant → deprecated`，
`buildCatalog` 也早已实现"退役状态退出路由索引"。但**没有任何机制能让一条条目走进那个状态**：

- 没有 `collab retire`；
- `supersedes` 字段从 frontmatter 一路投影到 JSON，但**没有任何规则消费它**；
- `AGREEMENT_LIMIT = 10` 已满，而它的计数**不带状态过滤** —— 于是错误提示里的
  "归档（status → dormant）腾位置"是**一句空头支票**：照做也解不开配额。

结果是条目只有一种死法：**被冷落**（0 引用、0 拦截）。

而这个死法有一个致命缺陷：**它同时命中两类完全不同的条目** ——

| 类型 | 例子 | 该不该退 |
| :--- | :--- | :--- |
| **已经成功的** | `domain-purity-is-structural`：2 次拦截，且已被 `DomainFrameworkFreeTest` 固化 | **该退** —— 内容活在测试里，不需要再被读 |
| **本来就没用的** | 写了很久但从未被引用、也说不出拦住了什么 | 该退 |

修剪需要一个能**区分这两者**的判据，而"0 引用"区分不了。这就是
`meta/known-gaps.md` 里"修剪政策的可达性扫描未自动化"长期开着的**真正原因** ——
不是差一个脚本，是**差一个判据**。

## 决策

新增一个 frontmatter 字段 `enforced`：

```yaml
enforced: <已把这条内容机械化的测试/工具/规则的路径>   # 非空 = 已毕业
enforced: null                                          # 未毕业（默认）
```

**非空 ⇒ 退出路由索引**（不进 `catalog.json`），与 `dormant`/`deprecated` 同一条路径。

### 关键区分：为什么不是"再删一个字段"

**`enforced` 与 `status` 是两条独立的轴**，含义不同：

| 轴 | 问题 | 值 |
| :--- | :--- | :--- |
| `status` | 这条**还成立吗**？ | `active` / `dormant`（过时） |
| `enforced` | 这条**还需要读吗**？ | `null` / `<path>`（已毕业） |

一条**正确且仍然有效**的条目可以毕业 —— 它没有过时，它只是**完成了**：
它的内容已经被写进某个测试，从此跑测试就遵守它，不需要任何人再读它。

生产实例已经存在于本库：`DomainFrameworkFreeTest` 把
[[patterns/domain-purity-is-structural]] 的规则变成了 surefire 的红绿判定。
**在那一天，那条条目就该毕业了。** 它没有，因为当时没有机制让它退。

### 判据共享

新增 `src/domain/entry/routed.ts` 的 `isRouted(fm)`，作为"是否占路由索引位"的
**唯一判据**：`buildCatalog`（哪条进 catalog）与 `collab new`（约定层配额还能不能加）
共用它。此前两者各写一份，导致配额说满了、路由表却对不上的自相矛盾。

## 后果

正面：

- 修剪终于有了**可判定**的判据："已被固化" —— 且它不依赖任何未经验证的测量。
  按 `enforced` 退役是**安全**的：内容活在测试里，退出索引不损失任何东西。
- `AGREEMENT_LIMIT` 的提示从空头支票变成可执行的 `collab retire <id>`。
- 代谢配额（新增 3 → 处理 1）有了自然触发点：**固化即产生一个退役名额**。
- 条目生命周期终点从"被冷落"变成"已完成"——知识库开始能够**收缩**。

负面：

- **新增字段 = 基座变更**（`meta/base-contract.md` §三）。已按三件套执行：
  本 ADR + `scripts/migrate-add-falsifier-enforced.mjs` + `collab validate` 归零。
- 121 条已有条目需要迁移（脚本补 `enforced: null`，一次全量、幂等、可重跑）。
- `catalog.json` 的结构未变（`enforced` 非空的条目直接**不出现**），
  所以对外消费方无感知。

## 替代方案

- **用 `supersedes` 复用**：否决。`supersedes` 是"我替代了谁"（向后指），
  `enforced` 是"我被什么固化了"（向前指，指向非条目产物）。方向相反，
  混用会让两个字段都不可读。
- **自动毕业**（`enforced` 有值即自动 dormant）：否决。判断"某个测试真的固化了
  这条内容"比判断"这条过时了"更容易错，更高的不可逆性配更重的确认门 ——
  CLI 强制 `--confirm`，且 `enforced` **永不修改 `status`**。
- **不新增字段，用 `status: dormant` 表达毕业**：否决。那会把"过时"与"完成"
  压成同一个状态，于是**两条轴退化回一条**，`pruning-policy` 的四分类也就失去了意义。

## 关联

[[meta/base-contract]] [[meta/pruning-policy]] [[patterns/domain-purity-is-structural]] [[patterns/policy-without-mechanism]] [[ADR-0009-id-是不可变快照]] [[meta/evolution-log]]
