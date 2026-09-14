---
id: S10
type: skill
status: draft
created: 2026-09-11
updated: 2026-09-11
domains: [meta, tooling]
applies-to: [W6]
supersedes: null
author: heiniao
aliases: [S10]
---

# S10 collab CLI 使用

## 上下文

AI 输出 patch 后，需要一条可靠路径把它转成 commit、branch、PR，且不暴露写权限给 AI。

## 问题

- 手动复制粘贴易漏、易漂移。
- AI 直接 push 有安全风险。
- 缺少 YAML 校验和双向链接检查。

## 方案

**状态**：草案，待工程化落地。

### 命令设计

```bash
# 初始化本地仓库与 profile
collab init --profile alice

# AI 输出 patch 到 .collab/proposed/
# 用户确认并生成 commit
collab propose "<描述>"
collab apply --confirm

# 校验（YAML/链接/反面章节/_index 一致性）
collab validate

# 推送到个人 fork
collab push

# 发起 PR（约定级自动带 RFC 链接）
collab pr --to upstream

# 从上游同步，保留本地 profile
collab sync --rebase --respect-profile
```


### 技术选型建议

- Node.js + Commander 或 Rust + clap。
    
- YAML 解析用 `yaml` / `serde_yaml`。
    
- 链接检查：正则 + 文件存在性。
    
- 不依赖 git 库，直接调用 `git` 命令。
    

### 落地顺序

1. `init` + `apply`（最小可用）。
    
2. `validate`。
    
3. `push` + `sync`。
    
4. `pr`（调用 GitHub API）。
    

## 反面

- 不要在 CLI 里存 git token。
    
- 不要让 `apply` 跳过 `validate`。
    
- 不要让 `sync` 覆盖本地 profile。
    

## 关联

[[A7-distribution-and-community]] [[W6-local-patch-to-community-pr]] [[S11-profile-declaration]] [[profiles/_index]]