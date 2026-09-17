---
id: S1
type: skill
status: active
created: 2026-09-11
domains: [meta]
applies-to: [all]
supersedes: null
author: heiniao
aliases: [S1]
updated: 2026-09-17
provenance: 2026-09-16 吸收 A1-output-format —— 两条讲的是同一件事（H2 输出），逐字重复
trigger: 回答格式不统一；或不知该不该用 H2 起头
---

# S1 H2 输出

> 本条已吸收 `A1-output-format`（原约定层）。按 `agreements/` 的新定义，
> **输出格式是"怎么做"的手册，不是协作规则** —— 归 skills。

## 上下文

与 AI 协作需要统一输出格式。

## 问题

回答以任意形式开头，破坏结构一致性。

## 方案

- 任何回答的**第一个元素必须是 `##` 二级标题**。
- 标题之前不得有任何文字、空行或符号。
- 正文全部位于 H2 之下。
- 若需分节，可用 H3/H4，但**顶级结构始终是 H2**。

## 反面

- 不要在 H2 前加任何"好的""收到"之类的客套话。
- 不要用 H1 —— 保持与对话历史的层级一致。

## 关联

[[ROOT]] [[W2-three-stage-analysis]] [[A10-review-前置原则]]
