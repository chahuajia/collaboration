---
id: evolution-loop
type: pattern
status: active
source: 生物进化论
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [evolution-loop]
trigger: 知识库只增不减开始腐烂；或缺变异/选择/遗传任一环；或经验只留在对话里带不走
---

# 进化闭环

## 上下文

知识库需要持续演化，否则会腐烂。

## 问题

- 无变异 = 无法适应新情况。
- 无选择 = 冗余膨胀。
- 无遗传 = 经验流失。

## 方案

- **变异**：技能可自由新增。
- **选择**：定期修剪，见 `meta/pruning-policy.md`。
- **遗传**：约定是稳定基因，双方确认才能改。
- **生命周期**：draft → active → dormant → deprecated。

## 反面

- 不要无节制新增。
- 不要害怕删除。
- 不要让约定频繁变异。

## 关联

[[patterns/entropy-reduction]] [[meta/pruning-policy]] [[ROOT]]