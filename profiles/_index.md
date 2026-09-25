# Profiles 索引

每位协作者在此目录下维护 `<username>.yaml`，声明侧重点与排除项。

## 使用

- 复制 `templates/profile-template.yaml` 为 `profiles/<你的用户名>.yaml`
  （**模板只有一处** —— 本行原写着"复制 `_template.yaml`"，而那份文件不存在。
  2026-09-26 更正；同一条指令在 `README.md` 的"快速开始"里是对的）。
- 填写 `identity` / `focus` / `exclude` / `fork-policy`。
- AI 回答会读取此文件。（`collab sync` **尚未实现** —— 见 [[S10-collab-cli]]）

## 规则

- Profile 是偏好声明，不覆盖约定。
- 与 Git 身份冲突时，以 Git 为准。
- 不写敏感信息。

## 当前 profiles

| 文件          | 人                           |
| :---------- | :-------------------------- |
| [[heiniao]] | 黑鸟（偏好：中文 commit/注释、H2 开头回复） |

## 关联

[[cli-agent-boundaries]] [[S10-collab-cli]] [[S11-profile-declaration]] [[S1-h2-output]]
