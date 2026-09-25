---
id: base-contract
type: meta
status: active
created: 2026-09-16
updated: 2026-09-26
author: heiniao
aliases:
  - base-contract
provenance: 2026-09-16 一个会话内改了四次基座（链接规则 / 目录拓扑 / id 规则 / new 的行为），四次都没有迁移脚本
---

# 基座契约（冻结）

## 为什么要有这一页

自举系统**必须先有固定核心**，上面的东西才敢长（见 `patterns/self-bootstrapping-requires-fixed-core`）。

而这个仓库在 **2026-09-16 一天之内改了四次基座**：

| 改动 | 后果 |
| :--- | :--- |
| 链接解析：id → 文件名 → id（来回一次） | 全库 601 条引用被重写 |
| 目录拓扑：新增 `integrations/` | 工具加了一个 kind，17 个文件改引用 |
| id 规则：确立"不可变 + 必须是文件名前缀 + 必须进 aliases" | 新增两条验收 |
| `collab new` 增加约定层配额 | 改变了"能做什么" |

**四次都没有迁移脚本**，全靠 `collab validate` 事后兜底。
这在"只有一个人 + 一个 agent"的规模下侥幸没出事，但它不是设计，是运气。

## 冻结的内容

### 一、目录结构

```
agreements/      workflows/      skills/      patterns/
integrations/    meta/           meta/decision-records/
domains/         rfcs/           profiles/    templates/   inbox/
```

> **这份清单的权威在代码里**：`collab-cli` 的 `CONTRACT_DIRS`（`src/application/contractDirs.ts`）。
> `collab validate` 会在**未声明的顶层目录里出现文件**时报 `UNDECLARED_DIR` ——
> 换句话说，**想加一个新目录，你必须先改基座**，改不了就说明它不该加。
>
> 文档指向代码、而不是各存一份清单：这是"单一真相源"在本页的落地。

### 二、kind ↔ 目录 ↔ id 前缀

| kind | 目录 | id 前缀 |
| :--- | :--- | :--- |
| agreement | `agreements/` | `A` |
| workflow | `workflows/` | `W` |
| skill | `skills/` | `S` |
| pattern | `patterns/` | 无 |
| adr | `meta/decision-records/` | `ADR-` |
| integration | `integrations/` | 无 |

### 三、frontmatter 必填字段

`id` · `type` · `status` · `created` · `updated` · `author` · `aliases` · `enforced`

> `enforced` 为 `string | null`（**可空但必填**，对齐 `supersedes`）：
> 非空 = 该条目已被测试/工具固化，**退出路由索引**（见 [[ADR-0011]]）。
> 缺失它不会解析失败（parser 有 `.default(null)`），但新条目由 `collab new` 发出。

### 四、id 规则（不可变快照）

1. **永不重编号** —— 缺口即死亡记录（`ADR-0009`）
2. **必须是文件名的前缀**：`<id>.md` 或 `<id>-<slug>.md`（`ID_FILE_NAME_MISMATCH`）
3. **必须出现在 `aliases` 里** —— 渲染层靠 alias 解析 id 形式的链接（`ID_NOT_IN_ALIASES`）
4. **编号是身份，语义后缀是描述** —— 后缀可改，编号不动

### 五、链接规则

1. **推荐 id**：`[[S12]]` —— 不可变，改名不失效
2. **也接受文件名**：`[[S12-边界解析]]` —— 更可读，改名要跟着改
3. **围栏与行内代码里的双括号不算链接**（`extractLinks`）

### 六、生成物

| 文件 | 谁生成 | 过期后果 |
| :--- | :--- | :--- |
| `_index.md` | `collab index` | `MISSING_FROM_INDEX` / `DANGLING_INDEX_ENTRY` |
| `catalog.json` | `collab catalog`（`apply` 自动刷新） | `CATALOG_STALE` |

## 变更规则（破坏性变更）

**动上面任何一条 = 破坏性变更。** 必须同时具备三样，缺一不可：

1. **ADR** —— 写清动机、后果、替代方案（`meta/decision-records/`）
2. **迁移脚本** —— 放**工具链仓**（`collab-cli/scripts/one-off/`），可重跑、可审计
   （见 `skills/S30-批处理脚本骨架`）。**本仓不放脚本** —— 它保持纯文档
   （2026-09-26 更正：原写 `scripts/migrate-*.mjs`，而脚本早已迁出本仓，指的是一个不存在的目录）
3. **`collab validate` 归零** —— 它是唯一验收；引用面清不干净就是没改完

> **不做迁移脚本的代价不是"麻烦"，是"下一个会话不知道发生过什么"。**
> 规则可以靠文档传递，**变形的过程只能靠脚本传递**。

## 关联

[[ADR-0009-id-是不可变快照]] [[meta/naming-conventions]] [[meta/pruning-policy]] [[patterns/self-bootstrapping-requires-fixed-core]] [[patterns/reproducible-verification]] [[S30-批处理脚本骨架]]
