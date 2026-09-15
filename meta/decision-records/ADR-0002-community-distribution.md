---
id: ADR-0002
type: adr
status: accepted
date: 2026-09-11
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [ADR-0002]
---

# ADR-0002 采用社区化分发模型

## 背景

COLLABORATION v3 是个人知识库。用户希望分享出去，让更多人机协同参与演化，每人可有选择性和侧重点。

## 决策

1. 升级为公开仓库，许可证 CC BY-SA 4.0。
2. 采用三层分发：本地 / fork / 主干。
3. 身份三层：Git + YAML + Profile。
4. 门槛分层：约定 RFC，工作流 PR，技能 CI。
5. AI 边界：不持主干写权限，不自动 push。
6. 分阶段落地：先文档（v4），再 CLI（S10），再 GitHub Action（W6 工程化）。

## 后果

正面：
- 社区可参与演化，个人可选择性订阅。
- 归因清晰，演化可追溯。
- AI 安全边界明确。

负面：
- 治理复杂度上升。
- 冷启动风险：可能无人 fork。
- 需要维护 CLI 和 CI。

## 替代方案

- 保持私有：否决，无法社区化。
- 直接自动 push：否决，安全与历史污染风险。
- 一步到位上 GitHub Action：否决，先文档后工程化更稳。

## 关联

[[A7-distribution-and-community]] [[W6-local-patch-to-community-pr]] [[W7-rfc-process]] [[S10-collab-cli]] [[S11-profile-declaration]] [[patterns/distributed-evolution]]