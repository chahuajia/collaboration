---
id: cli-agent-boundaries
type: integration
status: active
created: 2026-09-11
updated: 2026-10-06
applies-to:
  - cli-agent
author: heiniao
aliases:
  - cli-agent-boundaries
  - A7
provenance: 2026-09-16 由 A7-distribution-and-community 拆出：只保留"AI 写权限边界"这一半（可执行、今天在用）；社区治理那半面向 0 人社群，已归档（见 ADR-0007）。2026-10-06 第一次修订：从"不 commit"改成"可 commit（带门禁 + 署名 + 任务分支）、不 push、不 merge 主干"。同日第二次修订：人问"禁止 merge 分支是不是一刀切"——查下来规则**从未**禁止任务分支之间 merge，缺的是**"主干"的定义**与**执行面**，故补定义、补"允许 merge 任务分支"的显式行，并把"没有仪器"如实记为缺口
trigger: AI 有写权限不知边界；或不该 commit/push 却做了
enforced: null
---

# cli-agent：写权限与身份边界

> 原 `A7-distribution-and-community` 装着两半：**① AI 的写权限边界**（本条目）、
> **② 社区治理**（分发分层、RFC 门槛、许可证）。
>
> **关键修正**：原文写"AI 永远不持有主干写权限" —— **这不是事实**。
> 有文件权限的 agent 确实持有写权限。**一条无法执行的规则会稀释全部规则的权威**，
> 所以改成可执行、且能自查的版本。

## 上下文

AI 在这个环境里**有**文件读写与 git 权限。所以问题不是"要不要给权限"，
是"**有权限时怎么用**"。

## 问题

- 规则说"AI 没有写权限"，事实相反 → 规则被无视，且每轮都在违反它。
- AI 直接 `commit` / `push`：commit 历史被噪音污染，上下文注入的代价被放大。
- 没有身份机制：改动无法归因，演化无法追溯。

## 方案

### 一、行为边界（可执行）

> **2026-10-06 修订**：原文是"**AI 不 commit、不 push**"。改了。
>
> **为什么改**：长任务里 agent 需要**切分支 / 合并 / 出错回滚** ——
> 而"改动只留在工作区"让 git 历史看不出过程、回滚只能靠人手工还原，协作者也读不到中间状态。
>
> **为什么原来那两条理由不构成反对**：本条目自己写的反对理由是
> "**commit 历史被噪音污染**" + "**没有身份机制：改动无法归因**" ——
> 两条都**可机制化**，不是"commit 本身危险"：
> 噪音 → **一任务一分支**；归因 → **`Generated-by:` 署名**。
>
> **顺带修掉一处"承诺 vs 现实"**：原来说"AI 不 push"，但 CLI 的 `collab push` **谁都能跑**
> （规则只写在散文里）。现在：CLI 侧 **默认拒绝**（要 `--allow-push` / `COLLAB_ALLOW_PUSH=1`），
> MCP 侧**根本没有 push 工具**。

| 动作 | 边界 |
| :--- | :--- |
| 读写工作区文件；`git status` / `log` / `diff` | 允许 |
| **建分支 / 切分支** | 允许，但**一任务一分支** |
| **commit** | 允许 —— 但**必须走 `collab commit`**（先 validate）+ **署名**（`Generated-by:`） |
| **merge 任务分支 → 任务分支** | ✅ **允许**（长任务集成 / 出错回滚 / 让协作者看见过程 —— 与"允许 commit"是同一条理由） |
| **merge 到保护分支（主干）** | ❌ 人的动作 —— 「保护分支」见下 |
| **`git push`** | ❌ **默认不**。需人显式授权：`collab push --allow-push`，或环境变量 `COLLAB_ALLOW_PUSH=1` |
| 持有远端凭据 | ❌ 不要在 AI 会话里传 token |

### 「保护分支（主干）」是什么

**"主干"必须先有定义 —— 没有定义，这条规则既不能执行、也不能自查。** 默认名单：

1. **远端默认分支**（`git symbolic-ref refs/remotes/origin/HEAD`，通常是 `main` 或 `master`）；
2. `main` / `master`（无论远端怎么配）；
3. `release/*` 一类发布分支 —— **由各仓自己声明**；本条目只定义上面两类（各仓要加，就在该仓的边界文件里加，不要改这里）。

**为什么"主干归人"**：主干是**选择**发生的地方（[[patterns/distributed-evolution]]：变异在边缘、选择在主干、遗传靠 PR）。
merge 进主干是一次**不可回滚的公开表态** —— 它把一堆变异一次性固化成共识。
这件事的价值全在判断，而判断正是 agent 证据最弱的一环（[[usage-guide]] 的三条诚实说明）。

**为什么任务分支之间可以 merge**：它是**过程**，不是表态。
长任务里 agent 需要把 A 分支合进 B 分支接着做；禁掉它，回滚只能靠人手工还原、协作者也读不到中间状态 ——
那正是 2026-10-06 把"不 commit"改成"可 commit"的同一个理由（见本条目开头的修订记录）。

**一句话**：**AI 可以 commit（走 `collab commit`、带署名、在任务分支上），也可以在任务分支之间 merge；但不 push、不 merge 到保护分支（主干）。**

**自证方式**（两条都可查）：

- 机器提交**可数**：`git log --format=%b | grep -c '^Generated-by:'`
- 主干上**没有** agent 的直接 push：`git log --format=%an main` 只应出现人
- 主干上**没有** agent 的 merge：`git log --merges --format='%h %s' main` 只应出现人的合并 ——
  **但这条现在只能靠人主动去查**，见下节。

### 执行面：这道门已经接线（2026-10-06）

按 [[patterns/policy-without-mechanism]] 三问自查（**接线后**）：

| 问 | 现状 |
| :--- | :--- |
| 违反能否 60 秒内被外部观察到？ | ✅ 一条命令：`collab guard-push --branch <b>` |
| 发现之后有强制动作吗？ | ✅ 拒绝（exit 1），且远端一个字节不动 |
| 执行面具备吗？ | ✅ 两层 —— CLI（`collab push`）+ git hook（`pre-push` → `collab guard-push`） |

**三条线，各管一段**：

| 谁 | 管什么 | 怎么用 |
| :--- | :--- | :--- |
| `collab push` | 走 CLI 的推送 | `collab push --allow-push --allow-protected` —— **两把钥匙**：前者"我可以推"，后者"我可以推主干" |
| `pre-push` hook → `collab guard-push` | **绕过 CLI 的 `git push`**（hook 是 git 自己调的，躲不开） | 由 `collab init` 生成；git 从 **stdin** 把待推 refs 交给它 |
| 远端 branch protection | 兜底（`--no-verify` 那类绕过） | 在 Gitee / GitHub 上开 —— **这是唯一拦得住"人手动绕过"的一层** |

**主干名单**（`collab push` 与 `guard-push` 共用同一份判据 —— 抽成了纯函数，不是两处各写一遍）：

1. 内置 `main` / `master`；
2. **远端默认分支**（`<remote>/HEAD` 指向的那条 —— 你的仓可以叫 `trunk`）；
3. git 根的 `.collab-protected-branches`（一行一个 pattern，`*` 通配，如 `release/*`）——
   **各仓自己声明，且进版本控制**；
4. 环境变量 `COLLAB_PROTECTED_BRANCHES`（CI / 一次性）。

**只做并集**：保护只能被增加，不能被某个来源悄悄取消。要放行只有一条路 —— 显式授权。

**删除远端分支同样受管**（`pre-push` 里 `local ref` = `(delete)` 也拦）：删 `main` 比推它更狠。

**仍然没有机制的那一层**：**"merge 到主干"本身**。`git merge` 是本地操作，`collab` 刻意不包装它
（包一层只会被绕过）。它现在的实际拦截点是**它之后的 push**：远端保护 + `pre-push`
—— 换句话说，一次本地 merge 只会在**推上去的那一刻**被拦。
想更早拦，可以在仓里加 git 的 `pre-merge-commit`（那是各仓自己的 hook 设置，不在本工具范围）。

### 二、身份三层

| 层 | 来源 | 作用 |
| :--- | :--- | :--- |
| Git 身份 | `git config user.name` / `user.email` | 真相来源，可验证 |
| YAML 元数据 | 条目头的 `author` / `provenance` | 归因与溯源 |
| 未确定 | `AI`（AI 产出） | — |

### 三、归因

- `author` 默认取 **`git config user.name`**（`collab new` 已实现）。
- `provenance` 必须保留来源，**禁止洗稿**。

## 反面

- 不要把"AI 不 commit"读成"AI 没有权限" —— 前者是行为约束，后者是错误陈述。
- **不要在没有分支纪律时 commit** —— 一任务一分支是"允许 commit"的**前提**；
  在主分支上连提交，就是原条目反对的那种"噪音污染"。
- 不要把 `--allow-push` / `COLLAB_ALLOW_PUSH` 当成默认值写进 agent 的环境 ——
  它是**人的**授权动作，不是配置项。
- 不要在 AI 会话里传入远端 token。
- 不要为"以后有社区"先写好治理流程 —— 那是"从未来设计"（0 人时无人执行）。

## 关联

[[A6-version-authority]] [[S10-collab-cli]] [[chatgpt-paste-protocol]] [[patterns/policy-without-mechanism]] [[patterns/distributed-evolution]] [[usage-guide]] [[known-gaps]]
