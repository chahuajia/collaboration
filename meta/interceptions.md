---
id: interceptions
type: meta
status: active
created: 2026-09-15
updated: 2026-09-17
author: heiniao
provenance: ROOT 的"选择压力"缺了收益侧测量——没有它，演化只会优化"便宜"
aliases: [interceptions]
---

# 拦截账本

## 上下文

条目库的成本是可见的（每次都要加载、裁决、冲突），收益是不可见的（"这条救了我一次"没人记）。
**只被惩罚、不被奖励的性状，必然朝"更便宜"的方向漂**——这是 79KB bundle 的成因。

## 问题

- 没有一个数字能回答"这套规范到底值不值"。
- 修剪没有客观依据：不知道该留哪条、删哪条。
- 新条目生来就没有适应度信号，只能靠"写得挺用心"存活。

## 方案

### 落点（防淹没）

本文件是 **跨项目判决索引**，不是项目运行日志。详见 [[patterns/project-evidence-vs-kb-ledger]]。

| 写这里 | 写业务仓 |
| :--- | :--- |
| 一行：条目 · 拦住了什么 · 链接 | 轮次报告全文、候选 interceptions |

L2 撞墙 → 先 `working-memory/interceptions-candidates.md`；W4 通过后再追加本表**一行**。

### 记账格式

每次某条目**真实拦住了一个错误**，追加一行：

### 样例（**不是**真实记录）

| 日期         | 条目                       | 拦住了什么                              | 如果不拦，后果       | 证据                    |
| :--------- | :----------------------- | :--------------------------------- | :------------ | :-------------------- |
| 2026-09-15 | [[S5-null-not-sentinel]] | 用 `(0,0)` 表达"无坐标"，导致"赤道 0 度"被误判为无值 | 定位功能在赤道附近静默失效 | <commit / 对话 / issue> |

### 真实记录

**当前条数：7。**

| 日期         | 条目                              | 拦住了什么                                                        | 如果不拦，后果                            | 证据                                                                                                                           |
| :--------- | :------------------------------ | :----------------------------------------------------------- | :--------------------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| 2026-09-16 | [[S13-Smart-Constructor]]       | 差点把「至少 1 块 AVAILABLE 才能换出」写成 **Station 构造不变量**               | 空站 / 全在充电的站无法存在；配电池与真实运营被构造器挡掉     | `evolutionary` 第 4 轮：症状表「不变量放哪一层」→ S13；不变量落在 `Station.swap`。见 `evolutionary/specs/round-4-report.md` P5                      |
| 2026-09-16 | [[dependency-decision]]         | 差点在薄应用层一出现就引入 **Spring Boot**                                | 无具体问题却上框架；测试变重；领域被间接污染风险           | 第 5 轮 Q2 三问书面否定；`pom.xml` 仍仅 JUnit。见 `evolutionary/specs/round-5-report.md`                                                  |
| 2026-09-16 | [[domain-purity-is-structural]] | 差点把 Spring 注解/import 放进 domain 或 application                 | 「分层」变成口号；领域测被迫起容器                  | 第 6 轮引入 Spring 后加 `DomainFrameworkFreeTest` 钉死；34 测绿。见 `evolutionary/specs/round-6-report.md` R5                             |
| 2026-09-16 | [[domain-purity-is-structural]] | 差点把 `@Entity` 直接标在领域 `Station` 上                             | 领域模型绑死 JPA；换仓储/换 ORM 牵动核心；架构测试形同虚设 | 第 7 轮改用 `StationJpaEntity` 映射；架构测试扩禁 `jakarta.persistence`/`org.hibernate`；35 测绿。见 `evolutionary/specs/round-7-report.md` J4 |
| 2026-09-17 | [[patterns/frontend-ddd-rsc]] | 差点继续换电首页全 `use client`+`useEffect` 拉站列表 | 首屏空、与 `/credit` RSC 不一致、无服务端读模型 | `topic/fe-ddd-rsc` `b20f899`：RSC+station-gateway+swap 岛 |
| 2026-09-17 | [[S34-边界层与领域的错误翻译]]             | 差点继续用 `msg.startsWith("unknown station")` 在 Controller 判 404 | 改仓储文案即错 HTTP；边界重复「解析」领域消息，S34 反面   | 第 14 轮：`UnknownStationException` + `SwapApiErrorTranslator`；41 测绿。见 `evolutionary/specs/round-14-report.md` T4               |
| 2026-09-17 | [[S34-边界层与领域的错误翻译]] | 差点复用站级/笼统 Commerce 翻译器把 ENTITLEMENT_* 映射成 409，或用 message 嗅探判状态码 | 权益用户不匹配被客户端当冲突重试；改文案即错 HTTP；S34 反面 | evo-collab-extreme 切片4：EntitledSwapApiErrorTranslator 按 DomainErrorCode→422/409；EntitledSwapControllerTest 3/0；evolutionary 90b4e89+89426a7 |
| 2026-09-18 | [[patterns/policy-without-mechanism]] | 差点继续只堆 extreme v5–v7 条文、不装 worktree/回执/会计，并宣称「已按最新门禁优化」 | 集群净吞吐低于单主轴；docs/派工假繁荣；feat 滞后 | evo-collab-extreme 复盘；v8 起机制入库；见 `patterns/policy-without-mechanism.md` |

### 判据

- **必须具体**：写"拦住了什么"，不写"很有用"。
- **必须有证据**：commit、报错、对话片段——没有证据的不算。
- **可以不记**：不是每次协作都有拦截；记流水账会让这个文件自己变成新的熵。

### 怎么用

| 用途 | 做法 |
| :--- | :--- |
| 修剪 | 长期 0 拦截、0 引用的条目 → 标记-清除候选（见 `meta/pruning-policy.md`） |
| 提升 | 拦截 ≥ 3 次的条目 → 说明它高频承重，值得写得更硬、更靠前 |
| 分发 | 对外介绍这套规范时，拿拦截记录当证据，而不是拿条目数量 |

## 反面

- 不要把它做成"功劳簿"——它是**测量工具**，不是绩效表。
- 不要编造：一条假的拦截记录会让整个账本失去意义。
- 不要为了凑数而记录"我提醒了要注意 X"这种没有后果的提醒。

## 关联

[[A4-proactive-update]] [[W4-three-question-retro]] [[W5-update-collaboration]] [[meta/pruning-policy]] [[meta/evolution-log]] [[patterns/evolution-loop]] [[patterns/project-evidence-vs-kb-ledger]]
