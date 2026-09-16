# 模式索引

跨领域借鉴的思想，作为结构与方法论的基础（40 条）。
**模式回答"为什么这样设计"**；具体做法在 `skills/`，不可谈判的规则在 `agreements/`。

## 结构与组织

| 模式 | 来源 | 用途 |
| :--- | :--- | :--- |
| [[patterns/rooted-graph]] | 图论 | 主结构：树 + 横切 + 双向链接 |
| [[patterns/faceted-classification]] | 图书馆学 | 多维检索 |
| [[patterns/law-three-tiers]] | 法律 | 三层结构（约定/工作流/技能） |
| [[patterns/load-bearing-vs-partition]] | 建筑 | 承重墙 vs 隔断（变更权限） |
| [[patterns/pattern-language]] | 建筑（Alexander） | 条目五段式格式 |
| [[patterns/adapter-internal-structure]] | 六边形架构 | 适配器内部结构自由 |
| [[patterns/hierarchical-actor-collaboration]] | FSM + HSM + Actor | 人机协作的层级 Actor |
| [[patterns/layer-vs-context]] | DDD + 六边形架构 | **层 ≠ 上下文** |
| [[patterns/domain-purity-is-structural]] | 水利工程 + 洋葱架构 | 领域纯洁性是结构性必然 |
| [[patterns/higher-order-factory]] | 高阶函数 + 框架设计 | 高阶工厂（回调注入） |
| [[patterns/what-how-are-projections]] | 投影几何 | What / How 是同一事物的两个投影 |

## 演化与社区

| 模式 | 来源 | 用途 |
| :--- | :--- | :--- |
| [[patterns/evolution-loop]] | 生物进化论 | 变异 + 选择 + 遗传 |
| [[patterns/entropy-reduction]] | 物理学（熵增） | 主动修剪 |
| [[patterns/ecological-niche]] | 生态学 | 适用边界 |
| [[patterns/catalyst-nodes]] | 化学（催化剂） | 杠杆节点 |
| [[patterns/distributed-evolution]] | 分布式系统 + 生物 | 社区演化 |
| [[patterns/peer-review]] | 学术同行评审 | 复核机制 |
| [[patterns/rfc-process]] | Rust RFC | 约定级变更 |
| [[patterns/horizontal-gene-transfer]] | 生物学 | fork 之间交换条目 |
| [[patterns/self-bootstrapping-requires-fixed-core]] | Lisp / Git / 哥德尔 | 自举必须固定基座 |
| [[patterns/knowledge-lifecycle]] | 生态学（营养循环） | 知识的多层循环 |
| [[patterns/two-dimension-knowledge]] | 城市规划 + 图书馆学 | 成熟度 × 范围 |

## 知识与上下文

| 模式 | 来源 | 用途 |
| :--- | :--- | :--- |
| [[patterns/context-overflow-solved-by-retrieval]] | 数据库 / 检索 | 溢出的解法是检索，不是寄放 |
| [[patterns/three-layer-memory]] | 认知心理学 + 存储分层 | 工作记忆 / 进度 / 长期知识 |
| [[patterns/cross-domain-borrowing]] | A9 | 跨域借鉴五步法 |
| [[patterns/naming-as-definition]] | 语言哲学 | 命名即定义 |
| [[patterns/qian-systems-engineering]] | 钱学森《系统工程》 | 总体设计部 |

## 工程判据

| 模式 | 来源 | 用途 |
| :--- | :--- | :--- |
| [[patterns/design-decision]] | A8 | 设计决策三问 |
| [[patterns/dependency-decision]] | 供应链（Make-or-Buy） | 引入三问 + 位置三问 |
| [[patterns/type-as-design]] | 类型论 | 类型即设计 |
| [[patterns/parse-dont-validate]] | 函数式编程 | 边界解析 |
| [[patterns/value-object-as-raw-material]] | 制造业 | 值对象是原料 |
| [[patterns/derivation-over-copy]] | 数据库规范化 | 派生优于复制 |
| [[patterns/allowlist-over-denylist]] | 安全工程 | 白名单优于黑名单 |
| [[patterns/structure-over-algorithm]] | 计算机科学（Wirth） | 结构优先 |
| [[reproducible-verification]] | 工程实践 | 可复现的证据优于手工验证 |
| [[waiting-is-a-decision-window]] | 人机协作实践 | 等待是决策窗口，不是空隙 |
| [[parallel-work-needs-delivery-proof]] | 分布式系统 + 人机协作 | 并行不是分派，是让每份分派可确认到达 |
| [[patterns/three-level-dry]] | 软件工程（DRY） | 不重复的三个层次 |
| [[value-semantics]] | 工程实践 | 值同不代表语义同（原 A11） |
| [[patterns/feature-discovery-over-hardcoded-paths]] | 航海导航 | 特征发现优于硬编码 |
| [[patterns/layered-defense]] | 军事纵深防御 | 分层拦截 |
| [[patterns/ooda-loop]] | 军事（OODA） | 复盘闭环 |
