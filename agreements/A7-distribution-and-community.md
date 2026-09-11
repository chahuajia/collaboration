---
id: A7
type: agreement
status: active
created: 2026-09-11
updated: 2026-09-11
applies-to: [all]
supersedes: null
---

# A7 分发与社区协作原则

## 上下文

COLLABORATION 从个人规范升级为社区协议，需要明确分发、身份、权限、演化边界。

## 问题

- AI 若持有 push 凭据，存在被上下文注入攻击的风险。
- 若每次对话都推 git，commit 历史会被噪音污染。
- 若没有身份机制，贡献无法归因，演化无法追溯。
- 若没有门槛，约定级内容可能被随意更改。

## 方案

### 1. 分层分发，AI 只操作本地

- **Layer 1（本地）**：AI 可以读写。
- **Layer 2（个人 fork）**：AI 不 push，由人触发。
- **Layer 3（社区主干）**：AI 不 push，通过 PR 进入。

**硬规则**：AI 永远不持有主干写权限，永远不自动 push。

### 2. 身份三层

| 层 | 来源 | 作用 |
| :--- | :--- | :--- |
| Git 身份 | `git config user.name/email` + GPG | 真相来源，可验证 |
| YAML 元数据 | 条目头部 `author` / `co-authors` / `focus` / `provenance` | 归因与溯源 |
| Profile | `profiles/<user>.yaml` | 声明个人侧重与排除 |

### 3. 变更门槛分层

| 层级 | 门槛 | 复核 |
| :--- | :--- | :--- |
| 约定 | RFC → FCP → PR → 2 人批准 | 必须 |
| 工作流 | PR → 1 人批准 | 必须 |
| 技能 | PR → CI 通过自动合并 | 可选 |
| 模式 | 同技能 | 可选 |
| 个人 fork | 自由 | 无 |

### 4. 分发模式渐进

- 阶段一（当前）：**手动 MD**，AI 输出完整文件。
- 阶段二：**本地 CLI**（S10），AI 输出 patch，人确认后 commit。
- 阶段三：**GitHub Action**，PR + CI + CODEOWNERS。

### 5. 许可证

建议 CC BY-SA 4.0：署名 + 相同方式共享。鼓励 fork，要求贡献回主干。

### 6. 隐私与归因

- 条目内 `author` 字段公开，贡献者需知晓。
- `provenance` 字段必须保留来源，禁止洗稿。

## 反面

- 不要让 AI 自动 push 到主干。
- 不要在 Layer 3 用 AI 直接 commit。
- 不要跳过 PR 审查合并约定级变更。
- 不要为追求"自动化"牺牲"人类最终决策"。

## 关联

[[A6]] [[W6]] [[W7]] [[S10]] [[S11]] [[patterns/distributed-evolution]] [[patterns/horizontal-gene-transfer]] [[ADR-0002-community-distribution]]