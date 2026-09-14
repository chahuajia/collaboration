---
id: W6
type: workflow
status: active
created: 2026-09-11
updated: 2026-09-11
domains: [meta]
applies-to: [community-contribution]
supersedes: null
author: heiniao
aliases: [W6]
---

# W6 从本地 patch 到社区 PR

## 上下文

复杂任务产生了值得贡献回主干的洞见，需要一条稳定的落地路径。

## 问题

- AI 直接 push 有风险。
- 手动复制粘贴易漂移。
- 缺少审查环节。

## 方案

1. **AI 输出 patch**：新文件内容或 unified diff，落到本地 `COLLABORATION/`。
2. **用户本地确认**：
   - 运行 `collab propose "<描述>"`。
   - CLI 生成 commit（含 YAML 校验、双向链接检查）。
3. **用户 push 到个人 fork**：
   - `collab push`。
4. **发起 PR 到主干**：
   - 走 W7（若为约定级）或普通 PR 流程。
5. **CI 校验**（工程化阶段）：
   - YAML 元数据完整。
   - 双向链接无死链。
   - "反面"章节非空。
   - `_index.md` 一致性。
6. **CODEOWNERS 复核**并合并。
7. **合并后 Bot 更新 evolution-log**。

## 反面

- 不要让 AI 直接 push。
- 不要跳过 CI 校验。
- 不要在 PR 里混入无关变更。

## 关联

[[A7-distribution-and-community]] [[W7-rfc-process]] [[S10-collab-cli]] [[patterns/peer-review]]