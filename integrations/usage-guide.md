---
id: usage-guide
type: integration
status: active
created: 2026-09-19
updated: 2026-09-19
aliases:
  - usage-guide
enforced: null
domains: []
applies-to:
  - all
author: heiniao
provenance: 2026-09-19 用户要一份使用指南；此前只有"入口文件"（AGENTS.md 指路）与"README"（是什么），没有"怎么用"的完整说明
falsifier: 会照着"知识库就该多读"的默认直觉去全量加载或每步都查，而不是先过"这个任务该不该查"那张表 —— 上下文税正是这么来的
trigger: 第一次用 collaboration；不知道该读什么/不该读什么；不确定某任务该不该查 KB；想给别的项目/别的 agent 接入
---
# COLLABORATION 使用指南

## 上下文

这份指南回答**"怎么用"**。`README.md` 回答"是什么"，`AGENTS.md` 是 agent 的入口，
`ROOT.md` 讲结构与演化机制。**三者不重复。**

它是什么（一句话）：**一个会收缩的决策知识库** —— 记录"做过什么决定、为什么、
什么条件下该推翻"，靠两个账本（拦截 / 缺口）和一套退役机制对抗腐烂。

**它不是**：产品文档、编码规范手册、agent 的人格设定、能自动让 agent 变聪明的东西。

## 问题

- **没有"怎么用"这一层**：入口文件只指路，读完之后"这个任务该不该查"仍要自己猜。
- **默认直觉是错的**：知识库看起来"多读总没错"，而实测恰恰相反 ——
  D 实验里读库的臂**更慢且零代码差异**。上下文税是真花掉的。
- **不知道什么时候该停**：查不到时，模型倾向于**编一条规范**而不是报告"找不到"。
- **接入别的项目时不知道带什么**：容易把 126 条全搬过去，而那些换个项目就是噪音。

## 方案

### 一、先决定：这个任务该不该查

**这是唯一重要的问题。** 查错的代价不是"多花几秒"，是读了一堆用不上的东西。

| 任务类型 | 查不查 | 理由 |
| :--- | :--- | :--- |
| **设计墙**：划边界、定不变量、选依赖、错误映射 | **查** | 有项目特有的决定，模型默认不知道 |
| **跨会话续进度** | **查**，但查**本仓 `working-memory/`**，不查 KB | 进度不属于长期知识 |
| **机械执行**：落库、改 import、补映射、批量重构 | **不查** | 对标上一切片 + 目标测绿即可 |
| **无人托管集群** | 查**1 条**（`extreme-unattended-cluster` 的机制表） | 要的是可执行字段，不是散文 |
| **一次问答 / 小修** | **不查** | 上下文税 > 收益 |

**判据（记这一条就够）**：

> **不读它，一个称职的模型会照着**本地哪个模式**做错吗？**
> 会 → 读；不会 → 不读。

注意问法是"会不会**照着本地代码**做错"，不是"知不知道正确答案"。
后者早已被证伪 —— D 实验六跑，模型自己就会把不变量放对地方。
**真正值钱的是拦住"照着眼前的坏代码继续写"。**

### 二、怎么查（三步，不要跳）

```
1. AGENTS.md 的「症状 → 条目」表      ← 第一路由，按"你要干什么"组织
2. 表里没有 → catalog.json 关键词检索   ← 它是目录，不是路由
3. 都没有 → 报告"找不到"，记 known-gaps ← 不要凭空发明规范
```

**第 3 步是设计，不是失败。** 库的判据写着：报"找不到"比编一条规范有价值。
实测两次（接手 evo 做 operator JPA、种子时机问题）都走了第 3 步，
结果都记进了 `known-gaps`。

**一次只读 1–2 条正文**，不要全量读。上下文是预算不是容器（`A16`）。

### 三、命令（唯一的执行面）

```sh
COLLAB=<collab-cli>/dist/cli/index.js
KB=<collaboration 目录>

node $COLLAB --dir $KB validate              # 全量校验（唯一验收标准）
node $COLLAB --dir $KB catalog               # 生成路由表 catalog.json
node $COLLAB --dir $KB new <type> [id]       # 建条目（draft）
node $COLLAB --dir $KB retire --candidates   # 列孤岛条目（只报告，不写盘）
node $COLLAB --dir $KB retire <id> --dormant --reason "<分类>: <证据>"
node $COLLAB --dir $KB retire <id> --enforced <测试路径> --reason "<分类>: <证据>" --confirm   # 毕业
```

> **选项的权威是 `collab <cmd> --help`**，本文只给范式。
> 2026-09-26 实测：`retire --enforced` 曾漏写 `--reason` 而 CLI 已强制要求它 ——
> **照文档抄就报错**。同一份命令清单写在两处，必然漂移；这里不再复述细节。

**`validate` 归零是唯一验收。** 但它验的是**结构一致性**
（链接 / 索引 / id / 目录 / 生成物），**不验内容有没有价值**。

### 四、写条目（三条门槛）

1. **`provenance`**：这条来自哪次真实事故/需求？**写不出来 → 它不该存在。**
2. **`falsifier`**（入库时必填）：「不读它，模型会照着本地哪个模式写错？」
   —— **模仿类**反事实，不是"模型不知道 X"。
3. **能变成机制就变成机制**：测试 > 工具约束 > 散文。

第 3 条最重要：**一条能写成测试的规则，写成文档是浪费**。
本库最值钱的条目 `domain-purity-is-structural` 已经**毕业** ——
它的内容活在 `DomainFrameworkFreeTest` 里，条目本身退出了路由索引。

### 五、三条诚实说明（用之前该知道）

1. **"读了有没有用"至今不可判定。** 唯一干净的对照实验（D 实验 r3）测出**零差异**
   且处理组更慢。账本里 8 条拦截**全是自述 near-miss，没有对照臂**。
   真正被证据支持的是**机制**（测试/工具/CI 那批），不是条目内容。
2. **拦截账本记的是"差点做错"，不是"读了才对"。** 这两件事差一个数量级。
3. **只有新条目有 falsifier 门槛**（2026-09-19 起）；之前 115 条没有。
   这意味着**老条目的承重性未被验证过**。

### 六、接入方式：MCP / CLI / 粘贴（三选一，都不是前提）

**先说清 `--dir` 指向什么**：它指向 **知识库工作区（KB workspace）**，**不是任意目录**。
判定标准只有一条：**`collab validate` 能读懂它** —— 有基座目录
（`agreements/ workflows/ skills/ patterns/ integrations/ meta/`）与根文档。

| 你想要 | 怎么得到 |
| :--- | :--- |
| 用**已有的**全局 KB（最常见） | 直接指向它，如 `D:\actto\front\project\collaboration_aggregate\collaboration` |
| 造一个**空 KB** | `collab init --profile kb --dir <path>` |
| 只给**项目侧**接入（不建 KB） | `collab init`（默认 `consumer`：生成入口 + WM 骨架 + 对全局 KB 跑校验的 wrapper） |

> **不需要把它做成 collaboration 的副本。** 你只要有**一个** KB ——
> 指向它即可。`collaboration` 是参考实现，不是前置条件。

**① MCP（支持 MCP 的客户端：Codex / Claude / Cursor）**

```bash
codex mcp add collab -- node <repo>/bin/collab.js mcp --dir <KB>
```

- 暴露 **6 个只读工具**：`catalog` / `read` / `search` / `validate` / `parse` / `apply_plan`
- **没有 commit / push** —— A7 的边界不靠叮嘱，靠**工具表里不存在那一项**
- **新开会话才生效**；KB 路径写在注册参数里，**KB 搬家要重新注册**

**② CLI 直调**（有 shell 的 agent）：`collab --dir <KB> validate` / `catalog` / `retire`

**③ 粘贴协议**（无 IO 的聊天窗口）：见 [[chatgpt-paste-protocol]] → `collab parse` → `collab apply`

> 三者等价，按环境选。**MCP 唯一的增量**是：不必把 `catalog.json` 塞进上下文，
> agent 可以直接按 id 取一条。

## 反面

- **不要**把它当系统提示全量加载 —— 126 条同时在场 = 没有规则（`A16`）。
- **不要**因为"库里有"就去读 —— 先过上面那张"该不该查"的表。
- **不要**在查不到时编一条规范 —— 报"找不到"并记 `known-gaps`。
- **不要**把项目进度写进 KB —— 那是 `working-memory/` 的活（`W10`）。
- **不要**把项目特有决定写成 pattern 再对外推 —— 换项目就是噪音。
- **不要**为了"有产出"建条目 —— 入库门槛在上一条。

## 关联

[[A16-上下文预算法]] [[W10-working-memory]] [[meta/pruning-policy]] [[meta/known-gaps]] [[meta/interceptions]] [[patterns/extreme-unattended-cluster]] [[ADR-0011]] [[S10-collab-cli]] [[patterns/project-evidence-vs-kb-ledger]]
