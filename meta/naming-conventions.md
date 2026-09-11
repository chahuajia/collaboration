# 命名规范

## 条目命名

- 约定：`A<n>-<kebab-name>.md`，如 `A1-output-format.md`
- 工作流：`W<n>-<kebab-name>.md`
- 技能：`S<n>-<kebab-name>.md`
- 模式：`<kebab-name>.md`
- 领域：`<domain>/_index.md`
- ADR：`ADR-<4位数字>-<kebab-name>.md`

## 内容命名

- 约定：以“必须/禁止/优先”开头
- 工作流：以“从 X 到 Y”命名
- 技能：动词 + 名词
- 模式：名词短语

## 链接

- 条目互链用 `[[ID]]`，如 `[[A1]]` `[[W2]]` `[[S5]]`
- 跨目录链接用 `[[patterns/rooted-graph]]`
## 社区相关命名

- Profile：`profiles/<username>.yaml`，username 用 git 用户名。
- RFC：`rfcs/RFC-<4位数字>-<kebab-name>.md`。
- CODEOWNERS：按目录划分，见 `.github/CODEOWNERS`。