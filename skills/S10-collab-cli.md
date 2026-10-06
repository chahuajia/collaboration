---
id: S10
type: skill
status: draft
created: 2026-09-11
updated: 2026-10-06
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
| `collab push [--allow-push] [--allow-protected]` | validate + `git push`。**默认拒绝**（远端归人）；推**保护分支**还要第二把钥匙 |
| `collab guard-push [--branch <b>]` | **保护分支门**：`pre-push` hook 调它（无参数时读 git 从 stdin 给的待推 refs）；`git push` 绕过 CLI 时靠它兜住 |
| `collab doctor` | **接线体检**：工作区 / 校验 / 生成物 / **入口有没有点名工具** / git（有 ❌ → exit 1） |

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
4. ✅ `parse` —— AI 粘贴输出 → `bundle.json`（`apply` 的进料口；块边界 = 条目自描述 frontmatter，见 [[ADR-0012]]）
5. ⬜ `propose` / `pr` / `sync`（`init` 已落地）—— 社区阶段的事，**等有第二个真实消费者**

## 反面

- 不要在 CLI 里存 git token。

- 不要让 `apply` 跳过 `validate`。

- 不要让 `sync` 覆盖本地 profile。

- **不要把未实现的命令写进"使用说明"** —— 文档承诺不存在的东西，和死链一样，是在教 AI 幻觉。
- 不要让同一个命令名承担两种含义（见上文的 `apply`）—— 改名，不改文档。
    

## 关联

[[cli-agent-boundaries]] [[W6-local-patch-to-community-pr]] [[S11-profile-declaration]] [[profiles/_index]]
## 实测坑（2026-10-06，本仓会话）

- **`## 关联` 必须用 `[[wikilink]]`**（本文件即范例）。写成"正文提到某路径"= 断链；跨库不存在的目标**不要建链接**，改为正文提及并说明"不在本共享库中"。
- **`index` 会给新条目留空单元格**（`| [[patterns/x]] |  |  |`）→ **必须补名字与摘要**；CLI 只在输出里警告一行，不补也不会红，容易漏 ✗。
- **`agreement` 有硬上限：10/10 已满**。满了 `new agreement` 直接拒收，并提示：先 `retire` 一条，**或把它改写成 工作流 / 模式 / 集成层**（"约定是承重墙，每加一条都在向未来每一次交互收税"）。实测：我把"上下文归属"判据改写为 `patterns/module-identity-before-layout` 才得以入库 ✓。
- **`pattern` 的必需章节是精确标题**：`## 上下文` / `## 问题` / `## 方案` / `## 反面` / `## 关联`。把括注写进标题（如 `## 问题（本仓实测）`）会被判 `MISSING_SECTION` ✗ —— **括注写在正文里**。
- **`--dir` 永远显式给**：本机存在用户级 `COLLAB_DIR`，裸命令会被它接管（把 `cwd` 语义夺走）→ 脚本 / hook / CI 里一律带 `--dir`。
