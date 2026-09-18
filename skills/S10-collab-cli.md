---
id: S10
type: skill
status: draft
created: 2026-09-11
updated: 2026-09-17
domains: [meta, tooling]
applies-to: [W6]
supersedes: null
author: heiniao
aliases: [S10]
trigger: 要用 collab CLI 落盘/校验；或不想给 AI push 权限
enforced: null
---

# S10 collab CLI 使用

## 上下文

AI 输出 patch 后，需要一条可靠路径把它转成 commit、branch、PR，且不暴露写权限给 AI。

## 问题

- 手动复制粘贴易漏、易漂移。
- AI 直接 push 有安全风险。
- 缺少 YAML 校验和双向链接检查。

## 方案

**状态**：**核心已落地**（`collab-cli`，6 个命令）。本节严格区分**已实现**与**设计草案** ——
把草案写成"使用说明"，等于教下一个 AI 去调用不存在的命令。

### 已实现（以 `collab --help` 为准）

| 命令 | 作用 |
| :--- | :--- |
| `collab validate [--json]` | 校验（frontmatter / 类型-目录 / 五段 / 死链 / 索引一致） |
| `collab new <type> [id]` | 从模板建条目 |
| `collab index [dir]` | 刷新 `_index.md`（保留人工列，只补不毁） |
| `collab apply <bundle.json>` | **把 bundle 落盘**：全有或全无；`--dry-run` / `--index` / `--commit` / `--json` |
| `collab commit -m "<msg>"` | validate + `git add` + commit |
| `collab push` | validate + `git push` |

> ⚠️ **语义变更**：`apply` 在早期草案里是"确认 AI 的 patch 并生成 commit"，
> 现在是"把 `bundle.json` 落盘"。**以本表为准**，草案作废。

### 设计草案（**未实现**）

以下命令**不存在**，任何"使用说明"都不得引用它们：

```bash
collab init --profile alice               # 初始化本地仓库与 profile
collab propose "<描述>"                    # AI 输出 patch 到 .collab/proposed/
collab pr --to upstream                   # 发起 PR（约定级自动带 RFC 链接）
collab sync --rebase --respect-profile    # 从上游同步，保留本地 profile
```

它们的共同前提是"有第二个消费者"（他人 fork / PR）——**今天还不是**。

### 技术选型建议

- **实际选择**：Node.js + TypeScript；`node:util` 的 `parseArgs`（未引入 Commander）。
- YAML 解析用 `yaml`；形状校验用 `zod`（**只在边界层**，领域层不 import 第三方）。
- 链接检查：正则 + 文件存在性。
- 不依赖 git 库，直接调用 `git` 命令。

### 落地顺序（已按现实修订）

1. ✅ `validate` —— 规则不可执行，后面全是空中楼阁
2. ✅ `new` / `index`
3. ✅ `apply` —— bundle → 工作区（全有或全无）
4. ⬜ `parse` —— A17 文本 → `bundle.json`（`apply` 的进料口）
5. ⬜ `init` / `propose` / `pr` / `sync` —— 社区阶段的事，**等有第二个真实消费者**

## 反面

- 不要在 CLI 里存 git token。

- 不要让 `apply` 跳过 `validate`。

- 不要让 `sync` 覆盖本地 profile。

- **不要把未实现的命令写进"使用说明"** —— 文档承诺不存在的东西，和死链一样，是在教 AI 幻觉。
- 不要让同一个命令名承担两种含义（见上文的 `apply`）—— 改名，不改文档。
    

## 关联

[[cli-agent-boundaries]] [[W6-local-patch-to-community-pr]] [[S11-profile-declaration]] [[profiles/_index]]
