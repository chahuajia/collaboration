---
id: module-identity-before-layout
type: pattern
status: draft
created: 2026-10-06
updated: 2026-10-06
aliases:
  - module-identity-before-layout
enforced: null
author: heiniao
---

# 模块身份先于目录（单仓库里"模块"如何获得身份）

## 上下文

一个仓里放多个应用时，**最容易犯的错不是写错代码，而是模块从未获得身份**：没有 workspace 清单、包名重复、共享包不被任何 manifest 声明、每个模块各自 lockfile。于是"跨模块改动"只能靠人肉，"搬迁文件"只能靠运气。

## 问题

本仓 2026-10-06 实测：

- 9 份 `package.json`，**0 份**声明 `@enatega/domain`；而**19 处**源码在 import/re-export 它 → 全靠路径别名悄悄解析。
- **3 个包同名 `enatega-frontend`**（web / admin / singlevendor-admin）→ workspace 根本无法建立。
- **7 套 `package-lock.json` + 7 份 `node_modules`** → 实测代价：`enatega-multivendor-api` 没有本地 `typescript`，验证其类型必须借 web 的编译器；直接 `npx tsc` 会**静默地没跑起来**（exit 1 但 0 条 `error TS`），曾因此白查一轮。
- **700 个文件搬迁失败过一次**：布局未定就搬 → 切分支后 web 应用回到仓库根，子目录只剩 30 个构建/IDE 残留，人以为"文件丢了"。

## 方案

1. **应用 ≠ 上下文**。`web` / `rider` / `store` / `admin` / `app` 是同一批业务上下文的**交付通道**；上下文按**业务能力**切：catalog · ordering · identity · logistics · support · payment · geo。**每个能力只允许一处权威实现**，其余模块只能**适配/消费**。
2. **身份先于目录**。先给模块**唯一名 + manifest + 显式依赖声明**，**再**谈目录搬迁。顺序颠倒 = 搬两遍（本仓已付过学费）。
3. **共享包必须被声明**。"靠路径别名解析的共享"在打包 / CI / 换机器时必炸；让**依赖图**解析，而不是让巧合解析。
4. **门禁分层**：app 内（`lint:layering`）→ 跨模块（依赖图、规则分叉检查）。后者**必须先有 workspace 才可能**——所以 workspace 是"跨模块门禁"的前置条件，不是洁癖。
5. **"一个仓放多个应用" ≠ 多模块**。判据 = **有 workspace 清单 + 唯一包名 + 被声明的共享依赖**；三者缺一，就还是"同居的独立项目"，只继承了单仓库的风险，拿不到它的收益。

## 反面

- 需要**权限硬隔离**、**独立发布节奏**、或模块间**几乎不共享领域概念** → 选多仓库，别硬塞进单仓。
- 上游形态可直接查（有网络时看根 `package.json` 与目录树）；**查不到就不要声称上游是 workspace monorepo**（本仓的推断曾被此教训纠正）。
- 反例：把"按应用切上下文"当成方案（web 上下文 / api 上下文…）→ 会让同一业务规则在 N 个应用里分叉，正是本条目要防的事。

## 关联

[[ROOT]] [[patterns/verify-with-independent-instruments]] [[skills/S10-collab-cli]] [[meta/pruning-policy]]

项目侧的 `agreements/A1-layer-ownership.md`（app 内部的层归属判据）是本条的**跨模块对应物**，它不在本共享库中，故以正文提及而不建链接。
