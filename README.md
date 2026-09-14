# COLLABORATION

我与 AI 协作的元规范。三层结构：约定（Agreements）、工作流（Workflows）、技能（Skills）。

## 三句话入门

- **约定**：必须遵守的协作规则，双方确认才能改。
- **工作流**：复杂任务的执行剧本，可复盘迭代。
- **技能**：可复用的能力单元，可自由增删。

## 如何查找

- **按层级**：见 agreements/ workflows/ skills/
- **按领域**：见 domains/
- **按模式**：见 patterns/
- **按状态**：每个条目头部 YAML 的 `status` 字段
- **按关联**：条目内的 `[[ID]]` 双向链接

## 如何使用

- 回答开头声明：本次遵守 A1/A2/A3，采用 W2，调用 S5/S7。
- 复杂任务结束：执行 W4 复盘三问。
- 出现新洞见：执行 W5 更新。

## 如何贡献

1. 识别内容属于约定 / 工作流 / 技能 / 领域 / 模式。
2. 用 templates/ 中对应模板创建文件。
3. 填写 YAML 元数据与双向链接。
4. 更新 `meta/evolution-log.md`。
5. 更新对应目录的 `_index.md`。
## 公开仓库

本规范已开源，欢迎 fork 与 PR。

- 许可证：CC BY-SA 4.0
- 主干：<待填写 GitHub 地址>
- 贡献流程：见 [[A7]] [[W6]]
- 约定级变更：见 [[W7]]
- 个人侧重点：见 [[S11]] profiles/

## 快速开始

1. Fork 主干。
2. 复制 `profiles/_template.yaml` 为 `profiles/<你的用户名>.yaml`。
3. 按 `focus` / `exclude` 声明你关心的部分。
4. 用 `collab` CLI（见 [[S10]]）或手动方式贡献。

## 演化机制

- 详见 `meta/pruning-policy.md`。
- 结构性决策写入 `meta/decision-records/`。