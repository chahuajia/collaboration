---
id: W10
type: workflow
status: active
created: 2026-09-13
updated: 2026-09-17
author: heiniao
aliases: [W10]
domains:
  - meta
applies-to:
  - all
supersedes: null
provenance: 跨对话上下文反复丢失——把"当前进度"混进知识库会稀释长期条目
trigger: 跨对话要续进度；或怕把临时状态混进知识库
enforced: null
---


## 上下文

跨对话的上下文会丢失——每次新对话都是白纸。但并非所有内容都值得进 COLLABORATION（长期知识）。
需要一层"工作记忆"作为桥梁（见 [[W10-working-memory]]）。

## 问题

- 用户重复解释进度，AI 重复建立上下文。
- 未完成的任务项、临时决策、当前阶段——无处安放。
- 把"当前进度"混进 COLLABORATION，会稀释长期知识库。

## 方案

### 目录结构

```text
working-memory/
├── README.md                 # 总览：当前活跃任务
├── todo.md                   # 未完成动作项
├── decisions.md              # 最近决策（可能变更）
└── _archive/                 # 阶段完成后的归档，永不修改
    └── YYYY-MM-DD-<task>.md
```

### 使用协议

| 角色 | 动作 |
| :--- | :--- |
| **AI 写** | 每轮复杂对话结束，主动更新当前进度（≤ 3 分钟） |
| **AI 读** | 用户说"继续 X"或"看进度"时，先读该文件 |
| **用户纠正** | 仅在发现错误时纠正——不主动维护 |
| **用户不读** | 它是"给 AI 的上下文"，不是给用户的作业 |

### 更新触发条件

**仅以下情况更新**：一个阶段结束、决策变更（"之前决定 X，现在改为 Y"）、发现新的未完成项、用户显式要求。

**不触发**：日常小改动、单行修复、纯查询。

### 归档规则

- 阶段结束后，当前进度快照 → `_archive/YYYY-MM-DD-<阶段名>.md`。
- 归档文件**永不修改**——它是历史记录。
- **有价值的洞见提炼进 COLLABORATION**，其余留在归档作参考。
- `_archive/` 超过 20 个文件 → 提示清理；`working-memory/` 总行数 > 500 → 强制归档一轮。

### 格式约定

**极简**——因为它要频繁读写。每份进度文档 ≤ 100 行：

```text
# <任务名> Progress
**更新**：YYYY-MM-DD HH:MM

## 当前阶段
## 已完成
## 进行中
## 下一步
## 遗留
## 最近决策
```

## 反面

- 不要让 `working-memory/` 变成第二个 COLLABORATION——它应该**频繁重写、频繁归档**。
- 不要每天更新——只在"阶段完成"或"决策变更"时更新。
- 不要归档而不提炼——归档是暂存，COLLABORATION 才是长期归宿。
- 不要让用户维护它——它是**给 AI 的上下文**。

## 关联

[[W5-update-collaboration]] [[A12-知识笔记返回]] [[A13-AI-入口文件规范]] [[A16-上下文预算法]] [[patterns/three-layer-memory]] [[patterns/context-overflow-solved-by-retrieval]]
