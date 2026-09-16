# 集成层索引

**本层定义**（2026-09-16）：**同一个协议，在不同环境下的操作手册。**

环境换了，这一层整条作废 —— 所以它们不应混进 `agreements/`（跨环境不变的协作规则）。

| 环境 | 名称 | 说明 |
| :--- | :--- | :--- |
| [[chatgpt-paste-protocol]] | 对话式 AI：粘贴交互 | AI 无 IO，靠用户传递与落盘 |
| [[chatgpt-output-format]] | 对话式 AI：输出格式 | 主体 vs 引用、三级分隔符、`===== FILE:` 协议 |
| [[cli-agent-boundaries]] | 有 IO 的 agent：写权限边界 | AI 有权限但不 commit / 不 push |

## 关联

[[A10-review-前置原则]] [[A14-多实体协作原则]] [[S10-collab-cli]] [[meta/naming-conventions]]
