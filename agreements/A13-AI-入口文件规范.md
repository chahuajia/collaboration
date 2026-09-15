---
id: A13
type: agreement
status: active
created: 2026-09-13
updated: 2026-09-13
applies-to:
  - all
supersedes:
author: heiniao
aliases:
  - A13
---

# A13 AI 入口文件规范

## 上下文

AI 进入项目时缺一个"去哪找知识"的入口——没有它，AI 要么盲目扫描，要么依赖用户反复解释。

## 问题

- 每次新对话是白纸，AI 不知道项目当前的焦点。
- 用户默默承担"告诉 AI 读什么"的负担。
- 没有入口，working-memory 和 COLLABORATION 都是"埋藏的宝藏"。

## 方案

### 单一入口：`AGENTS.md`

- **位置**：项目根目录。
- **命名**：`AGENTS.md`（跨工具通用，不锁厂商）。
- **长度**：50-100 行（薄入口）。
- **内容**：
  1. 项目一句话定位
  2. 阅读顺序（working-memory → COLLABORATION → 按需）
  3. 协作规则摘要（只列标题，不复制内容）
  4. 当前活跃任务指向
  5. 更新规则（本文件很少更新）

### 核心原则

**入口指向，不复制。**

- 它告诉 AI"读什么"，不告诉 AI"内容是什么"。
- 内容永远在 COLLABORATION / working-memory 里——单一真相源。
- 入口越薄越好——它是**导航仪**，不是**说明书**。

### 多工具演化

| 场景 | 处理 |
| :--- | :--- |
| 只有一个 `AGENTS.md` | ✅ 单一真相源 |
| 某工具需要专属文件（`.cursorrules`、`CLAUDE.md`） | 作为**局部补充**，写"详见 `AGENTS.md`"，不复制内容 |
| 局部与全局冲突 | **局部优先**——局部知道更多上下文 |
| 局部文件内容 | 只写"局部差异"，不重述全局 |
### 外部知识库引用

当 COLLABORATION 不在项目仓库内（独立仓库）时，`AGENTS.md` 必须明确声明：

- **外部知识库的地址**（git URL）。
- **读取方式**：
  - 有工具支持 → 自动读取。
  - 无工具支持 → 由用户粘贴关键文件。

**示例**：

```markdown
## 依赖的外部知识库

本项目的协作规范**不在本仓库内**——它在独立的 [COLLABORATION 仓库](https://github.com/chahuajia/collaboration)。

**AI 进入项目时的读取方式**：
- 如果支持 MCP → 通过 MCP Server 读取（见 ADR-0004）。
- 否则 → 由用户按"阅读顺序"粘贴关键文件。
```

## 反面

- 不要在入口里复制 COLLABORATION 内容——会产生两个真相源。
- 不要为每个 AI 工具各写一份完整入口——会打架。
- 不要让入口超 100 行——它变厚就失去了"入口"的意义。
- 不要忘记在入口里指向 working-memory——那是最重要的跨对话上下文。

## 关联

[[W10-working-memory]] [[A14-多实体协作原则]] [[patterns/three-layer-memory]]
