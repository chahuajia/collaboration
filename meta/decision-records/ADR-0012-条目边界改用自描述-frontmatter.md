---
id: ADR-0012
type: adr
status: accepted
date: 2026-09-26
created: 2026-09-26
updated: 2026-09-26
author: heiniao
aliases:
  - ADR-0012
enforced: null
falsifier: 会继续用"分隔符协议"（`===== FILE:`）做条目边界，于是每次都要为"模型又加了围栏/开场白/示例块"打补丁
provenance: 2026-09-26 用对话式 AI 生成条目，产出被围栏+开场白+协议示例污染，parse 直接拒收
---

# ADR-0012 条目边界改用自描述 frontmatter

## 背景

`parse` 从文本里切条目，靠的是人为分隔符（原 A17）：

```text
===== FILE: <path> =====
<完整内容>
===== END FILE =====
```

实测（2026-09-26）：对话式 AI 生成的一批条目，实际形态是

> **开场白 + ```text 围栏 + 真条目 + 协议示例块（`<path>` / `a.md`）+ 更多真条目**

它**遵守了分隔符**，却在外面加了自己的包装、还顺手把协议示例放进来了。

这不是模型不听话，是**约定的形状不对**：A17 定的是「分隔符长什么样」，
**没定「整段输出的形状」**。模型只会把"格式要求"当成它输出内容的一部分，
不会当成对整段输出的约束。

## 决策

**条目边界改用 frontmatter（`---` … `---`）—— 内容自描述，不依赖任何人为分隔符。**

| 旧 | 新 |
| :--- | :--- |
| 靠 `===== FILE: <path> =====` 定边界与路径 | 靠 YAML frontmatter 块定边界 |
| 路径随文本带来 | 路径**派生**：`EntryKindDir[type] + "/" + id + ".md"` |
| 块外有内容 = 拒收 | 块外散文**跳过并 warn** |
| 不合法块 = 整个拒收 | 不合法块**跳过并 warn**；一个合法块都没有才拒收 |
| `===== FILE:` 是必须的 | 保留为**可选冗余**（人能读，不再是契约） |

**为什么是 (a) 用 id 当文件名**：frontmatter 里只有 `id` 与 `type`，而 `id` 只是文件名**前缀**
（`idMatchesFileName` 只要求前缀）—— 从它无法唯一还原 `S36-agent-workspace-boundaries.md`。
选项 (b) 新增 `path:` 字段要动基座，(c) 读工作区复用 slug 会破坏 `parse` 的 domain 纯度。
取 (a)：派生确定、满足既有规则、不改基座。

## 后果

正面：

- 三种污染同时消失：块外散文/围栏（天然跳过）、协议示例块（被 id/type 规则挡掉）、`END FILE` 漏写（没有结束标记了）。
- 模型只需输出它**本来就在生成**的 frontmatter 条目，不再需要记住成对标记。
- `collab_parse` 从"已知对真实输出不可用"变回可用 —— 它是 MCP 里"agent 提议 → 人落盘"的唯一结构化通道（工具表只读，见 A7）。

负面：

- 只能解析**带 frontmatter 的条目**。任意文件（如 `src/foo.ts`）不再支持 —— 但那本来就不该走这条通道。
- 用 id 当文件名会**丢掉 slug**（`S36.md` 而非 `S36-agent-workspace-boundaries.md`）；
  slug 落盘后可由人补。

## 替代方案

- **继续加宽容解析**：否决。追不上——每个模型、每次会话的包装都不一样。
- **在约定里补一句"别加围栏"**：否决。那是散文，依赖模型自觉，正是本仓反复删掉的那类。
- **保留 `===== FILE:` 作为必须**：否决。它把"内容是否正确"与"包装是否合规"绑在一起，两者本该分离。

## 关联

[[chatgpt-output-format]] [[chatgpt-paste-protocol]] [[cli-agent-boundaries]] [[ADR-0009-id-是不可变快照]] [[patterns/derivation-over-copy]]