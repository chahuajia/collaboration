# templates —— 模板集中地

## 规则

**所有模板放在这里，一处一份。** 目录内不再放 `_template.*`。

历史上 `rfcs/_template.md`、`profiles/_template.yaml.md` 与 `templates/` 各存了一份，
结果是"改了一份、忘了另一份"。模板分散本身就是文档腐烂的入口。

| 文件 | 用于 | 目标位置 |
| :--- | :--- | :--- |
| `agreement-template.md` | 约定 | `agreements/A<n>-<name>.md` |
| `workflow-template.md` | 工作流 | `workflows/W<n>-<name>.md` |
| `skill-template.md` | 技能 | `skills/S<n>-<name>.md` |
| `pattern-template.md` | 模式 | `patterns/<kebab-name>.md` |
| `adr-template.md` | 结构性决策 | `meta/decision-records/ADR-<4 位>-<name>.md` |
| `rfc-template.md` | 约定级提案 | `rfcs/RFC-<4 位>-<name>.md` |
| `profile-template.yaml` | 个人侧重声明 | `profiles/<username>.yaml` |
| `evolution-log-template.md` | 演化日志追加 | 追加到 `meta/evolution-log.md` |
| `evolution-log-format.md` | ~~格式规范~~ | **已移到 `meta/evolution-log-format.md`**（它是规范，不是模板） |

## 唯一例外

`profile-template.yaml` 是 **`.yaml`**，不是 `.md`——它要被工具直接解析，不需要 Obsidian 索引。

## 一致性

模板的 frontmatter 字段 = `meta/naming-conventions.md` 规定的字段。
**改字段先改模板，再改条目**，否则模板会持续生产不合规的条目。

## 关联

[[S8]] [[S9]] [[W5]]
