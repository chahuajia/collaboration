---
id: chatgpt-paste-protocol
type: integration
status: active
created: 2026-09-13
updated: 2026-09-17
applies-to:
  - chatgpt-like
supersedes:
author: heiniao
aliases:
  - chatgpt-paste-protocol
  - A15
provenance: 2026-09-16 从 agreements/ 迁入 integrations/ —— 它假设"AI 无 IO、靠用户粘贴"，那只在对话式环境成立
trigger: 对话式 AI 无 IO 要粘贴；或逐文件粘贴/多轮猜需求
---

# chatgpt-like：粘贴交互协议

> 本条目原为 `A15-AI-交互协议`（约定层）。
> 2026-09-16 迁入集成层：它整条建立在"**AI 没有文件读写，IO 由用户承担**"这个环境前提上。
> 有文件权限的 agent（见 [[cli-agent-boundaries]]）不需要它。
## 上下文

对话式 AI 无法自主读写文件——IO 执行由用户承担。交互效率依赖用户的传递方式。

## 问题

- 逐文件粘贴耗时。
- AI "猜用户要什么"再问，来回多轮。
- 用户不知道"该给什么"。

## 方案

### 用户侧：一次上菜

**每次复杂对话开始**：

1. **先给 `AGENTS.md`**（一次）。
2. **AI 读完 AGENTS.md 后**，告诉用户"接下来需要读 X、Y、Z"。
3. **用户一次性粘贴** X、Y、Z（不逐轮）。

### AI 侧：先看地图再点菜

**AI 收到 AGENTS.md 后**：

- 先读"阅读顺序"的指向（working-memory / COLLABORATION）。
- 明确告诉用户"我需要哪几个文件"。
- 等用户提供后再开始分析。

**不猜测、不等待、不逐个问**。

### 传递原则

| 原则 | 说明 |
| :--- | :--- |
| **入口优先** | 复杂对话从 `AGENTS.md` 开始 |
| **增量传递** | 只传变化的部分（working-memory 的作用） |
| **按需读取** | 用 `_index.md` 精确定位，不全量 |
| **一次性传递** | 相关文件一次给，不逐轮 |
| **批处理写入** | AI 一次输出多个文件，用户一次写入 |

### 工具桥接

**方向**：采用 MCP Server（见 ADR-0004）。

**当前状态**：未实施。

**临时方案**：对于"一次要传多个文件"的场景，可写 `scripts/prepare-context.ps1` 打包。

## 反面

- 不要逐文件粘贴——用"打包"或"一次性"。
- 不要让 AI 反复问"还有别的吗"——AI 应先说清"总共需要什么"。
- 不要混用对话式和本地 Agent——按场景选择。

## 关联

[[A13-AI-入口文件规范]] [[A14-多实体协作原则]] [[W10-working-memory]] [[ADR-0004-采用-MCP-Server-作为工具桥接方案]]
