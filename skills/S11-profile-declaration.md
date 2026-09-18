---
id: S11
type: skill
status: active
created: 2026-09-11
updated: 2026-09-17
domains: [meta]
applies-to: [W6]
supersedes: null
author: heiniao
aliases: [S11]
trigger: 全量 fork 噪音大；或 AI 不知该为谁的侧重点优化
enforced: null
---
# S11 Profile 声明侧重点
## 上下文
社区演化中，每个人关心的领域不同，不需要全量订阅。
## 问题
- 全量 fork 导致噪音大、维护成本高。
- 无侧重点声明，AI 不知道为谁优化。
## 方案
每个协作者在 `profiles/<username>.yaml` 声明：
```yaml
#
identity:
  git: alice@example.com
  gpg: 0xABCD1234
focus:
  domains: [react, performance, security]
  workflows: [W1, W2]
  skills: [S2, S4, S5]
exclude:
  domains: [architecture]
  skills: [S7]
fork-policy:
  sync-upstream: weekly
  auto-prune-dormant: true
  contribute-back: true



```



### 用途

- 同步时按 `focus` / `exclude` 保留/裁剪条目。（`collab sync` **尚未实现** —— 见 [[S10-collab-cli]]）
    
- AI 回答时按 `focus` 调整深度与优先级。
    
- 主干的 `_index.md` 可统计 focus 分布，识别热点。
    

### 与 Git 身份的关系

- Git 身份是真相来源。
    
- Profile 是偏好声明，可以多份（工作/个人）。
    
- Profile 与 Git 不一致时，以 Git 为准。
    

## 反面

- 不要在 profile 里写敏感信息。
    
- 不要用 profile 绕过约定。
    
- 不要让 exclude 变成"我不看就不存在"。
    

## 关联

[[cli-agent-boundaries]] [[S10-collab-cli]] [[profiles/_index]]
