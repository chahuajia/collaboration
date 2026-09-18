---
id: multi-portal-capability-gate
type: pattern
status: active
created: 2026-09-18
updated: 2026-09-18
source: evolutionary 四端校正（消费者/商家/运营商/总后台）
author: heiniao
aliases:
  - multi-portal-capability-gate
  - P-portal-cap
trigger: 总后台批商家；运营商批下线；店主改商城；电池运维进错端；或四端角色串味
provenance: 2026-09-18 evo-collab-extreme——用户纠「operator=电池、merchant=商城、admin=平台批商家」；known-gaps 开「多门户×审批权」
enforced: null
---

# 多门户 × 能力门禁

## 上下文

同一业务系统常有**多个操作面**（消费者 App、商家后台、运营商后台、平台总后台）。
每个面只应暴露其**组织能力（capability）**允许的动作；把「审批权」挂在错误门户 = 假权限与角色串味。

本模式回答：**端（portal）与能力（capability）如何对齐，避免错端放权？**

## 问题

- **角色串味**：把运营商当成电池运维、把商家当成平台审批、把总后台当成店主工具。
- **错端放能力**：例——在运营商端做「批准商家入驻」（应属平台 admin）；在商家端批下线运营商。
- **IA 与权限脱节**：导航有入口，但后端 capability 未拒；或后端拒了但前端仍暗示「可以」。

## 方案

### 端 → 能力（默认映射 · 可按项目改名）

| 门户 | 默认可写能力 | 典型拒收 |
| :--- | :--- | :--- |
| 消费者 | 自身订单 / 换电 / 钱包读 | 任何组织审批 |
| 商家 | 商城 SKU / 券 / 店订单 | 入驻自批、下线审批、电池柜运维 |
| 运营商 | 套餐模板/覆盖、下线审批、站/柜运维 | 平台批商家入驻、跨商户商城写 |
| 总后台 / 平台 | 批商家入驻、跨组织策略 | 店内日常售卖操作（应回商家端） |

### 门禁三问（写入口前）

1. **这个动作挂在哪个门户？** 门户决定 IA，不决定「谁方便点」。
2. **需要的 OrgCapability / 角色是什么？** 与门户表不一致 → **拒收或改端**。
3. **成功路径是否可审计？** 跨组织审批必须有 append-only 审计（资源 id + actor）。

### 拒收句（写入 brief / PR）

> 错误端放错能力 = **拒收**；先改门户或改 capability，不「先通再补」。

## 反面

- 一个「万能运营端」塞入驻 + 商城 + 电池。
- 只改文案不改 capability 检查。
- 用「管理员」一词掩盖平台 vs 运营商边界。

## 关联

[[patterns/policy-without-mechanism]] [[patterns/extreme-unattended-cluster]] [[S36]] [[known-gaps]]
