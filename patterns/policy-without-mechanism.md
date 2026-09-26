---
id: policy-without-mechanism
type: pattern
status: active
created: 2026-09-18
updated: 2026-09-26
source: 人机协作实践（extreme v5–v7 政策堆砌负吞吐）+ 外源对照（隔离/回执/会计）
author: heiniao
aliases:
  - policy-without-mechanism
  - governance-theater
  - P-neg-opt
trigger: 加了规则/版本/门禁却更慢；优化像负优化；政策很多基础设施没有；docs 与派工热闹 feat 不涨
provenance: 2026-09-19 补「机制的最小形式 = 一根会亮的线」（4 个零执行政策 + 接线/假阳性两条）；2026-09-18 evo-collab-extreme——条文迭代快于 worktree/回执/会计落地，集群净效率低于单主轴
falsifier: 会照着「版本号递增 = 在进化」的既有惯性继续堆 vN 条文，不装 worktree/回执/会计（v5–v7 实测如此）
enforced: null
---

# 无机制的政策是负优化

## 上下文

协作库与集群编排都会「加门禁」：新版本、新 checklist、新反面句。
当**可观察机制**（隔离、超时强制接管、回执、会计、测绿凭证）未同步落地时，新增政策只增加**协调税与假进度面**，吞吐下降——这叫**负优化**，不是「还没优化够」。

本模式回答：**什么时候该停写条文、改装管道？**

## 问题

- **政策通胀**：同一模式 vN→vN+1 只加表行，不改执行面（同树多写、空 transcript、docs 当 HEAD）。
- **活动≠产出**：wake / agent id / `docs(wm)` / validate 绿被当成进度。
- **优化错层**：该上基础设施（worktree、Return contract、派出会计）时却只加「不要…」反面句。
- **负反馈延迟**：墙钟变长、feat/docs 比恶化，却仍宣称「已按最新门禁执行」。

## 方案

### 判定三问（任一「否」→ 停写政策，先装机制）

1. **本条规则的违反，能否在 ≤60s 内被外部观察发现？**（文件 / transcript / git / 测绿）
2. **发现后是否有强制动作？**（接管 / 拒收 / 停 wake / 改串行——不是「记一笔」）
3. **执行面是否已具备？**（worktree、回执模板、会计字段、目标测命令）——**没有则本条先不入库或标 `needs-mechanism`**

### 机制的最小形式：一根**会亮的线**（2026-09-19 补）

三问里的第 1 问其实是全部：**没有仪器，连"是否执行"都不可知。**

本仓实测的四个「从来没有执行过」的政策，共同点是**都没有仪器**：

| 政策 | 仪器 |
| :--- | :--- |
| 「引用计数 = 0 → 标记-清除候选」 | **无** —— `pruning-policy` 自认「从未有人测量」 |
| 「6 个月未被引用 → dormant」 | **无** —— 从未执行（v4.1.3 已废，理由正是"从未测量"） |
| 代谢配额「新增 3 → 处理 1」 | **无** —— 从未执行 |
| 「W4 通过后 harvest」 | **无** —— 连加几轮候选却**一次 W4 都没跑过** |

**2026-09-26 复查**：把 [[meta/pruning-policy]] 逐条对照仪器，又扫出 5 条同形状的
（分代、热 / 冷分级 + `archive/`、重叠 ≥ 50%、被 ≥ 3 个 fork 引用、某分支 ≥ 7 条建子目录）
—— 那张「机制成熟度」表是单一清单，**不在这里重抄**（同一事实写两处必然漂移）。

对照：**有仪器的，都立刻起了作用** ——

- `check-freshness` 接进 pre-push **当天**就抓出 2/3 仓已烂
- 候选池装上 `collab memory` 的龄期检查后，才能说"干净"，而不是"不知道"

⇒ **装机制的第一步不是写流程，是问：这件事有没有一根会亮的线？**

而**"写了脚本"不等于有线**：`check-freshness` 写于 09-16、接进 hook 是 09-18 ——
中间两天仪器存在但从未运行，一跑就抓出 2/3 仓烂。
**接线（谁在什么时候跑它）是仪器的一半。**

> ⚠️ 还有一条：**假阳性会让仪器被无视。**
> 候选池仪器首次实测报 4 条，全是嵌套 README（结构文档，不声称状态）——
> "稳定报警"的结果是没人再看它。判据宁可窄。

> ⚠️ **第三条（2026-09-26 补）：差值型仪器要先验"同源"，否则恒绿。**
> `check-freshness` 的判据是"自上次对账以来有几个提交"
> （`git rev-list --count <基准>..HEAD`）。**当 HEAD 是基准的祖先时，这个数返回 0** ——
> 实测：人在 **topic 分支**上对账（基准 `ac7aa6c`），之后切回更旧的 `main`（`dc3b366`），
> 仪器报"0 个提交，新鲜"；而账本覆盖的是**另一条分支**，对当前 checkout 什么都没说。
> 判据补成"**基准必须是当前 HEAD 的祖先**"（`git merge-base --is-ancestor`，退出码 0/1）。
> **推广**：凡是"与某个基准做差"的仪器，先问一句"基准和当前状态**同源**吗"——
> 不同源时，差值不是"零"，是"**没测**"。

### 政策 vs 机制对照

| 只写政策（负优化风险） | 须同 tick 落地的机制 |
| :--- | :--- |
| 「超时要接管」 | 探针 + 父写代码/重派的硬截止 |
| 「不要同文件双写」 | **worktree / 分支隔离**（[[S36]] L3） |
| 「要有交付证明」 | **feat commit + Tests run**；禁派出专用 docs |
| 「派了要对账」 | loop 头 `dispatched=N recovered=M` |
| 「沉默不算进行中」 | 禁预写 🔄；空 transcript = FAILED |
| 「L3 要动 KB」 | collaboration **条目/主表 diff**（[[patterns/pressure-routing]]） |
| 「别把日记写进账本」 | 分层落点（[[patterns/project-evidence-vs-kb-ledger]]） |
| 「要修剪」 | 可达性扫描/除名动作，而非从未执行的时间规则 |

### 入库门禁（防再堆 v9 空谈）

新增或大改 **流程/集群类**条目时，`evolution-log` 同行须含至少一项：

- 新**可执行产物**（模板、脚本、loop 字段、worktree 约定），或  
- **关闭**一条「有政策无机制」的 [[known-gaps]]，或  
- 显式写 `needs-mechanism: <缺什么>` 且**不**宣称已生效

### 与既有条目

| 条目 | 关系 |
| :--- | :--- |
| [[patterns/extreme-unattended-cluster]] | v8 起把隔离/回执/会计升为机制；本模式解释 **为何 v5–v7 曾负优化** |
| [[patterns/parallel-work-needs-delivery-proof]] | 「派出≠送达」的实例；本模式升到**政策层元规则** |
| [[patterns/pressure-routing]] | L1 绿当 L3 = 活动≠产出 |
| [[patterns/project-evidence-vs-kb-ledger]] | 日记进账本 = 假繁荣 |
| [[patterns/entropy-reduction]] / [[meta/pruning-policy]] | 「有修剪政策从未测量」= 同构负优化 |
| [[patterns/waiting-is-a-decision-window]] | 边界不清时加 agent = 负优化 |

## 反面

- 不要用「再写一版更严的门禁」代替 worktree / 回执 / 接管。
- 不要在 `needs-mechanism` 未清时把条目 trigger 写进 `AGENTS.md` 症状表冒充已生效。
- 不要把 evolution-log 的版本号递增当成协作能力提升。
- **不要把"写了脚本"当成"装了机制"** —— 没接线的仪器等于没装（`check-freshness` 空转两天的实例）。
- **不要让仪器稳定报假阳性** —— 那不是严谨，是让下一个人不再看它。

## 关联

[[patterns/extreme-unattended-cluster]] [[patterns/parallel-work-needs-delivery-proof]] [[patterns/pressure-routing]] [[patterns/project-evidence-vs-kb-ledger]] [[S36]] [[meta/pruning-policy]] [[patterns/entropy-reduction]] [[patterns/waiting-is-a-decision-window]] [[W4-three-question-retro]]
