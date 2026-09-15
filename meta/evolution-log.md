# 演化日志

| 日期         | 版本     | 变更                                                                                          | 提议者                  | 确认者            | commit | 关联                                           |
|:-----------| :----- | :------------------------------------------------------------------------------------------ | :------------------- | :------------- | ------ | -------------------------------------------- |
| 2026-09-11 | v1     | 初始 8 项 Skill 单文件                                                                            | chahuajia            | AI + chahuajia | -      |                                              |
| 2026-09-11 | v2     | 分层为约定/工作流/技能                                                                                | A4 触发                | AI + chahuajia | -      |                                              |
| 2026-09-11 | v3     | 升级为目录结构，引入模式语言、有根图、跨领域模式                                                                    | A4 触发                | AI + chahuajia | -      |                                              |
| 2026-09-11 | v4.0.0 | 并入社区分发模型                                                                                    | chahuajia提议 + ai双方确认 | AI + chahuajia | -      | A7, W6, W7, S10, S11                         |
| 2026-09-11 | v4.0.1 | 新增约定 A8（设计决策三问），立即生效                                                                        | chahuajia提议 + ai双方确认 | AI + chahuajia | -      | A8                                           |
| 2026-09-11 | v4.0.2 | 新增约定 A9（跨域借鉴五步法），立即生效                                                                       | chahuajia提议 + ai双方确认 | AI + chahuajia | -      | A9                                           |
| 2026-09-11 | v4.0.3 | 启用 evolution-log 新格式（提议者/确认者/commit/关联）                                                     | chahuajia提议 + ai双方确认 | AI + chahuajia | -      | A6                                           |
| 2026-09-12 | v4.0.4 | 新增约定 A10（Review 上移原则），立即生效                                                                  | chahuajia提议 + ai双方确认 | AI + chahuajia | -      | A10                                          |
| 2026-09-15 | v4.1.0 | **批量落盘**：A11-A16 / W8-W9 / S12-S30 / 17 个模式 / ADR-0003-0004 / 演化日志格式规范                      | AI（从 v4.1 bundle 提取） | heiniao（待确认）   | -      | A11-A16, W8, W9, S12-S30, ADR-0003, ADR-0004 |
| 2026-09-15 | v4.1.1 | 新增入口文件 `AGENTS.md`；建立 `inbox/` 增量策略；bundle 移出仓库根                                            | AI（提议）               | heiniao（待确认）   | -      | A13, A16                                     |
| 2026-09-15 | v4.1.2 | **模板集中化**：删除 `rfcs/_template.md`、`profiles/_template.yaml.md`；格式规范从 `templates/` 移到 `meta/` | AI（提议）               | heiniao（待确认）   | -      | templates/README.md                          |
| 2026-09-15 | v4.1.3 | **修剪策略升级**：从"6 个月一刀切"改为标记-清除 + 分代 + 冷热分级                                                    | AI（提议）               | heiniao（待确认）   | -      | meta/pruning-policy                          |
| 2026-09-15 | v4.1.4 | 修复 S5（反面/关联误复制自 S4）、S6（id 误写为 S4、domains 错）                                                 | AI（发现并修复）            | heiniao（待确认）   | -      | S5, S6                                       |
| 2026-09-16 | v4.1.5 | 新增 A16（上下文预算法）；创建 parking-lot.md / decisions.md / anchors.md；W12 暂缓 | AI（提议） | heiniao | -      | A16 |
