---
id: ADR-0004
type: adr
status: accepted
date: 2026-09-13
author: <待填>
created: 2026-09-15
updated: 2026-09-15
aliases: [ADR-0004]
enforced: null
---

# ADR-0004 采用 MCP Server 作为工具桥接方案

## 背景

对话式 AI 无法自主读写文件——IO 执行由用户承担。这导致交互效率依赖用户的传递方式。工具桥接的候选方案有三：

- **A. 打包脚本**：`scripts/prepare-context.ps1` 把多个文件拼成一个。
- **B. MCP Server**：实现 `collab-mcp-server`，让支持 MCP 的客户端直接访问 COLLABORATION。
- **C. 本地 Agent 工具**：Claude Code / Cursor / Aider——它们内置文件读写。

## 决策

采用 **B（MCP Server）**。

**理由**：

1. **协议标准**：MCP 是开放协议，一次实现，多处受益。
2. **能力完整**：Resources / Tools / Prompts 三类能力足以覆盖 COLLABORATION 的读写需求。
3. **生态成熟**：Claude Desktop / Claude Code / Cursor / Windsurf 等已支持。
4. **可复用**：MCP Server 可复用 `collab-cli` 的领域逻辑。

**实施时机**：

- **不在本 ADR 生效时实施**。
- **触发条件**：`collab-cli` 完成 `validate` / `new` / `index` / `commit` 四个核心命令后。
- **先决条件**：有真实的"在 Claude Code 或 Cursor 里工作"的场景。

## 后果

正面：
- 本地 AI 工具可以直接访问 COLLABORATION，不用手动粘贴。
- 统一接口，不需要每个工具自己实现访问逻辑。
- 复用 `collab-cli` 的领域逻辑。

负面：
- 增加一个独立项目的维护成本。
- 对"纯对话式 AI（当前界面）"无效——只对支持 MCP 的客户端有效。
- 需要跟踪 MCP 协议演进。

## 替代方案

- **A（打包脚本）**：否决——跨场景可移植性差；每换一个 AI 工具要重写。
- **C（本地 Agent）**：不否决——它和 B **互补**，不是替代。
  - 日常对话 → 对话式 AI
  - 大量代码修改 → 本地 Agent
  - 中间态（读 COLLABORATION）→ B（MCP Server）

## 关联

[[chatgpt-paste-protocol]] [[A13-AI-入口文件规范]] [[A14-多实体协作原则]]
