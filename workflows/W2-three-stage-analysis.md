---
id: W2
type: workflow
status: active
created: 2026-09-11
updated: 2026-09-17
domains: [all]
applies-to: [any-technical-question]
supersedes: null
author: heiniao
aliases: [W2]
trigger: 复杂技术问题要结构化分析；或怕只答表面无根因
---

# W2 复杂问题分析三段式

> 本条已吸收 `A2-depth-and-reflection`（2026-09-16）。
> 按"agreements = 协作规则"的定义，**分析方法不是协作规则** —— 它是执行剧本，归 workflows。

## 上下文

技术问题需要结构化分析，避免想到哪说到哪。

## 问题

- 只回答表面，无根因。
- 无横向关联。
- 无防御性建议。

## 方案

三段式：

1. **纵深剖析**：是什么 → 为什么 → 如何避免。
2. **横向扩展**：关联场景、系统性风险、跨领域借鉴。
3. **防御性架构**：分层拦截、检查清单、可观测性。

**主动指出用户可能没意识到的关联问题** —— 相关但没问的，也要说。

## 反面

- 不要在简单问题上强行三段。
- 不要为扩展而扩展，关联必须真实。
- 不要只给结论不给推理。

## 关联

[[patterns/layered-defense]] [[W3-ddd-refactor]] [[ROOT]]
