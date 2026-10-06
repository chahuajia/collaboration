---
id: refactor-slice-done-criteria
type: pattern
status: draft
created: 2026-10-04
updated: 2026-10-06
author: heiniao
aliases:
  - refactor-slice-done-criteria
  - 一个重构切片算不算完成
trigger: 说不清一次重构切片算不算做完；或准备用"测试全绿""代码变短"宣布完成
provenance: 2026-10-04 从 food-delivery 项目的十多个真实重构切片反推（原文件 `agreements/A2-definition-of-done.md`，原本就是 draft；该仓本地库已解散）；2026-10-06 迁入共享库，状态仍为 draft，待人确认
falsifier: 会照着"测试全绿 + 代码变短就算完成"的默认直觉收工 —— 该项目有一处改动落在没有任何测试会执行到的 util 上，测试全绿什么也没证明
enforced: null
---

# 一个重构切片算不算完成

## 上下文

没有判据时，"这轮算完成了吗"每次都要靠临场感觉回答，
而临场感觉最容易被两件事骗到：**测试变绿** 与 **代码变短**。

## 问题

- "测试全绿"只能证明**被测试覆盖到的部分**没变；覆盖面之外它什么也没证明。
- "两处代码合并成一处"可能是一次静默的行为变更（实测到两例：时间格式化、终态状态集合）。
- 没有判据时，缺口账本与进度记录会各写各的，下一个人无法判断哪条路已经走完。

## 方案

一个切片完成，需要**同时**满足四条：

| # | 判据 | 怎么验（可执行） |
| :--- | :--- | :--- |
| 1 | **规格先于实现** | 看 diff / 提交顺序：先有描述"应该是什么"的测试或规格，再有生产代码改动（优先级见 [[A10-review-前置原则]]） |
| 2 | **等价性声明是分级的、可测量的** | 明说档位：逐位相等 / 容差相等（给数字）/ 仅记录分歧；禁止笼统的"行为不变"（做法见 [[characterization-first-refactor]]） |
| 3 | **验证覆盖了改动本身** | 全绿**且**：改动若落在无测试覆盖的文件上，必须另有证据（类型检查过滤 / grep 复核 / 定向脚本）。"没测到"不等于"没问题"（根因见 [[patterns/tests-encode-assumptions]]） |
| 4 | **账记下来了** | 进度写项目 `working-memory/`；新缺口按归属写（通用判据 → KB，项目缺口 → 项目 WM）；行为变更必须显式（改预言机 + 写明理由） |

### 判据本身的验证

- **1** 可数：每个切片都有对应的规格文件。
- **2** 可查：容差必须有具体数值（实测用过"逐位"与"1e-9 km"两档）。
- **3** 可查：对无覆盖文件跑 `tsc --noEmit` 过滤，或 grep 复核断言。
- **4** 可数：进度与账本有对应条目。

## 反面

- 不要把"测试通过"当成"没改行为"：未覆盖的改动必须另有证据。
- 不要把"代码变短"当成完成：删掉一份活副本可能换掉语义（先数消费者，见 [[rule-placement-by-layer]]）。
- 不要为了凑完成而合并**不等价**的实现：那是行为变更，应先裁决再改。
- 不要在最后一个切片上就地宣布**整体**完成：整体完成还要求"缺口账本里的记录要么关闭、要么明确挂着"。

## 关联

[[A10-review-前置原则]] [[characterization-first-refactor]] [[patterns/tests-encode-assumptions]] [[patterns/verify-with-independent-instruments]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[patterns/project-evidence-vs-kb-ledger]]
