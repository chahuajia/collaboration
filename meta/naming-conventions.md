---
id: naming-conventions
type: meta
status: active
created: 2026-09-11
updated: 2026-09-17
author: heiniao
aliases: [naming-conventions]
---

# 命名规范

## 条目命名

- 约定：`A<n>-<kebab-name>.md`，如 `A1-output-format.md`
- 工作流：`W<n>-<kebab-name>.md`
- 技能：`S<n>-<kebab-name>.md`
- 模式：`<kebab-name>.md`（**语义 id**；见下「模式短别名」）
- 领域：`<domain>/_index.md`
- ADR：`ADR-<4位数字>-<kebab-name>.md`

**硬规则**：文件名里**不允许出现空格**。空格会让链接无法按文件名匹配
也会让脚本与 shell 处理变复杂（历史上 `A9 ...md`、`A10 Review ....md` 就是这样断链的）。

### 模式短别名（与 A/W/S 对齐的检索面）

模式层**保留语义文件名作 `id`**（跨 fork 可合并，见下表）。为降低与 `A10`/`S5` 的检索落差：

1. `aliases` **必须**含 `id`（已有强制）。
2. **新建/改触**模式时，`aliases` **建议**再加一个短别名：`P-<short-kebab>`（如 `P-ledger`），供症状表与口语引用。
3. **禁止**为补齐短名而批量重命名旧文件（[[ADR-0009-id-是不可变快照]]）；旧条按需改触时补 aliases。
4. **不**把 `P-<n>` 位置编号定为模式主键——避免与语义 id 双真相；位置编号留给 A/W/S。

## 内容命名

- 约定：以“必须/禁止/优先”开头
- 工作流：以“从 X 到 Y”命名
- 技能：动词 + 名词
- 模式：名词短语

## 链接

**推荐用 id 作链接锚点** —— 它不可变（[[ADR-0009-id-是不可变快照]]）。

- **推荐**：`[[S5]]`（id）—— 语义后缀改名后**链接存活**
- **也可以**：`[[S5-null-not-sentinel]]`（文件名）—— 更可读，但改名后要跟着改
- 跨目录链接用 `[[patterns/rooted-graph]]`（完整路径也合法）
- **`id` 必须出现在 `aliases` 里** —— 渲染层（Obsidian）靠 alias 解析 id 形式的链接。
  缺失 → 静默断链。由 `ID_NOT_IN_ALIASES` 强制，**不靠自觉**。
- **`id` 必须是文件名的前缀**：文件要么叫 `<id>.md`，要么叫 `<id>-<slug>.md`。
  由 `ID_FILE_NAME_MISMATCH` 强制。

## 元数据字段（frontmatter）

| 字段 | 必填 | 说明 |
| :--- | :--- | :--- |
| `id` | 是 | 条目的唯一标识，与文件名前缀一致 |
| `type` | 是 | agreement / workflow / skill / pattern / adr / meta |
| `status` | 是 | draft / active / dormant / deprecated（ADR 用 proposed/accepted/…） |
| `created` / `updated` | 是 | ISO 日期 |
| `author` | 是 | git email 或 `heiniao` |
| `aliases` | 建议 | 填 `[<id>]`，让 Obsidian 也能按 ID 解析双链 |
| `provenance` | 建议 | **这条目来自哪次真实事故/需求**。写不出来，说明它不该存在（见 pruning-policy） |
| `enforced` | 是（可空） | 已把这条内容机械化的**测试/工具/规则路径**。非空 = 已毕业，**退出路由索引**（见 [[ADR-0011]]）。不知道就填 `null` |
| `falsifier` | 入库时 | 不读它，模型会照着**本地哪个模式**写错？必须是**模仿类**反事实 —— 不是"模型不知道 X"（D 实验六跑已证伪后者） |
| `applies-to` / `domains` / `supersedes` | 按类型 | 见各自的模板 |

## 模板位置

**所有模板集中在 `templates/`，一处一份。** 目录内不放 `_template.*`。
详见 `templates/README.md`。

## ID 的两种形态（演化方向）

| 形态 | 例子 | 优点 | 缺点 |
| :--- | :--- | :--- | :--- |
| **位置编号**（当前） | `A10`、`S5` | 短、好写 | 两个 fork 的 `S5` 可能是不同东西，**跨国杂交会撞号** |
| **语义名**（模式层已用） | `rooted-graph`、`null-not-sentinel` | 可跨 fork 合并、可读 | 稍长 |

模式的命名已经采用语义名，这是正确方向。A/W/S 目前仍是位置编号；
**若要真正支持"fork 之间交换条目"（见 `patterns/horizontal-gene-transfer`），
早晚要补上语义别名**——最小改动是在 `aliases` 里同时写编号与语义名。
## 社区相关命名

- Profile：`profiles/<username>.yaml`，从 `templates/profile-template.yaml` 复制。
- RFC：`rfcs/RFC-<4位数字>-<kebab-name>.md`。
- CODEOWNERS：按目录划分，见 `.github/CODEOWNERS`。
