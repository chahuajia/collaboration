---
id: ADR-0001
type: adr
status: accepted
date: 2026-09-11
author: heiniao
---

# ADR-0001 采用 v3 目录结构

## 背景

v2 单文件 `COLLABORATION_SKILLS.md` 把约定/工作流/技能混编，无法支撑长期演化。

## 决策

升级为目录结构 `COLLABORATION/`，三层分离，引入模式语言、有根图、YAML 元数据、生命周期、ADR。

## 后果

正面：
- 可横切检索，可按领域/状态/工作流多维查找。
- 条目有生命周期，可修剪。
- 结构性决策有记录。

负面：
- 文件数量增加，维护成本上升。
- 需要自律执行修剪。

## 替代方案

- 保持单文件：否决，无法支撑演化。
- 最小可用版（先跑两周）：用户明确否决，选择完整版。

## 关联

[[ROOT]] [[patterns/rooted-graph]] [[meta/pruning-policy]]