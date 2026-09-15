# 工作流索引

| ID                                 | 名称                | 触发场景          |
| :--------------------------------- | :---------------- | :------------ |
| [[W1-blank-page-triage]]           | 页面空白/数据不出来的黄金排查路径 | 前端数据异常        |
| [[W2-three-stage-analysis]]        | 复杂问题分析三段式         | 任何技术问题        |
| [[W3-ddd-refactor]]                | DDD 架构重构工作流       | 目录膨胀、模块耦合     |
| [[W4-three-question-retro]]        | 复盘三问              | 复杂任务结束        |
| [[W5-update-collaboration]]        | 更新 COLLABORATION  | 出现新洞见         |
| [[W6-local-patch-to-community-pr]] | 从本地 patch 到社区 PR  | 社区贡献          |
| [[W7-rfc-process]]                 | RFC 流程            | 约定级变更         |
| [[W8-规格优先的-AI-协作流程]]               | 规格优先的 AI 协作流程     | 需求清晰、要 AI 写实现 |
| [[W9-Spike-工作流]]                   | Spike 工作流         | 问题空间不明确、写不出测试 |
| [[W10-working-memory]]             | 工作记忆维护流程          | 跨对话的分阶段任务     |

## 选择顺序

| 你要做的事             | 用哪个                                                       |
| :---------------- | :-------------------------------------------------------- |
| 需求清楚，要让 AI 落地实现   | [[W8-规格优先的-AI-协作流程]]（内含 [[A10-review-前置原则]] 的 review 上移）  |
| 需求不清楚，连"要做什么"都还没定 | 先 [[W9-Spike-工作流]]，产出规格后回到 [[W8-规格优先的-AI-协作流程]]           |
| 出了故障要定位           | [[W1-blank-page-triage]]                                  |
| 任务做完了要沉淀          | [[W4-three-question-retro]] → [[W5-update-collaboration]] |
