# 架构领域

## 相关条目

- [[W3-ddd-refactor]] DDD 重构工作流
- [[S7-ddd-frontend-light]] DDD 前端轻量化
- [[patterns/frontend-ddd-rsc]] 前端 BC / RSC
- [[patterns/shared-kernel-across-bc]] 跨 BC 共享内核
- [[patterns/domain-purity-is-structural]] 领域纯洁性
- [[patterns/layer-vs-context]] 层 ≠ 上下文
- [[patterns/load-bearing-vs-partition]] 承重墙 vs 隔断
- [[patterns/rooted-graph]] 有根图
- [[patterns/extreme-unattended-cluster]] 极端无人托管集群
- [[patterns/policy-without-mechanism]] 无机制的政策是负优化
- [[patterns/pressure-routing]] L1/L2/L3 压测落点

## 症状 → 先读

| 症状 | 条目 |
| :--- | :--- |
| 跨包 import「看起来不对」但能编译 | [[patterns/shared-kernel-across-bc]] |
| 领域层碰了框架 | [[patterns/domain-purity-is-structural]] |
| 压测 KB 却只改 CLI 测试 | [[patterns/pressure-routing]] |
| 多 agent 无沙箱 / 空转 | [[patterns/extreme-unattended-cluster]] · [[S36]] |
| 加规则反而更慢 / 政策不落地 | [[patterns/policy-without-mechanism]] |

## 常见问题

- 目录扁平化膨胀
- 模块耦合 vs 共享内核
- 状态管理分层（服务端读模型 vs UI 瞬态）
