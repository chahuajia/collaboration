---
id: S4
type: skill
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [react]
applies-to: [W1]
supersedes: null
author: heiniao
aliases: [S4]
---


## 上下文

组件渲染期间调用 setState，触发无限循环或报错。

## 问题

`Cannot call setState during render`。

## 方案

按场景选择：

| 场景 | 修复 |
| :--- | :--- |
| 用户交互触发 | 移至事件处理函数 |
| 副作用（props 变化同步） | 用 `useEffect` |
| 可推导值 | 渲染期间直接计算，不用 state |
| prop-state 同步（罕见） | 存储上一次的值，条件化更新 |

## 反面

- 不要在渲染期间无条件 setState。
- 不要用 useEffect 处理可推导值。

## 关联

[[W1-blank-page-triage]]  [[domains/react/_index]]