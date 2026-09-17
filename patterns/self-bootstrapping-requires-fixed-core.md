---
id: self-bootstrapping-requires-fixed-core
type: pattern
status: active
source: Lisp / Git / 哥德尔不完备定理
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [self-bootstrapping-requires-fixed-core]
trigger: 设计"一切皆插件/条目"却找不到启动点；或核心也能被自己改；或自举陷入无限递归
---

# 自举需要固定核心

## 上下文

设计"一切皆 X"的自演化系统（如"一切皆插件"、"一切皆条目"）。

## 问题

- 直觉上，"一切皆 X"意味着 X 可以被 X 描述。
- 但这会导致**无限递归**：X 的定义需要 X，X 的 X 需要 X……
- 最终系统无法启动——因为**没有起点**。

## 方案

### 核心原则

**越"万物皆 X"的系统，越需要一个极小、极稳定、不可被 X 描述的 X 本身。**

### 先例

| 系统 | "一切皆 X" | 不可自举的核心 |
| :--- | :--- | :--- |
| Lisp | 一切皆 S-表达式 | **Reader 本身是手写的** |
| Git | 一切皆对象 | **Object 格式由 SHA-1 + zlib 定义** |
| Vue | 一切皆 DOM | **Vue Runtime 不是 Vue 组件** |
| React | 一切皆组件 | **React Runtime 不是 React 组件** |
| Spring | 一切皆 Bean | **BeanFactory 本身不是 Bean** |

### 应用方法

1. 识别"要自举什么"（如：条目、组件、Bean）。
2. 明确"自举的基座是什么"（如：schema、格式约定、不变量）。
3. **基座必须：极小、极稳定、不可自描述**。
4. **其余一切**才能在该基座上自举。

### 应用到 COLLABORATION

- **要自举的**：条目（A/W/S/Pattern）。
- **基座**：
  - 目录结构（`agreements/`、`workflows/`、`skills/`、`patterns/`）
  - YAML frontmatter 字段（id/type/status/created/updated/author）
- 双向链接语法（双方括号包裹条目 ID）
  - `_index.md` 的约定
- **基座不进化为条目**——它是"元语法"，是约定本身。

## 反面

- 不要追求"一切皆条目"——先想清楚不可自举的核心是什么。
- 不要用"递归"作为设计——递归需要**基例**。
- 不要让基座频繁变动——它应该是最稳定的部分。

## 关联

[[patterns/design-decision]] [[A10-review-前置原则]] [[patterns/context-overflow-solved-by-retrieval]]
