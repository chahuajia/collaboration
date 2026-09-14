---
id: horizontal-gene-transfer
type: pattern
status: active
created: 2026-09-11
updated: 2026-09-14
source: 生物学（水平基因转移）
author: heiniao
---

# 水平基因转移

## 上下文

不同 fork 之间可以直接交换有价值的条目，不必经过主干。

## 问题

- 所有贡献都走主干，效率低。
- 有价值的局部经验可能被主干忽略。

## 方案

- 允许 fork 之间直接 cherry-pick 条目。
- 交换时保留 `provenance` 字段，标明来源。
- 若该条目被多个 fork 引用，触发"提升为独立技能"（见 pruning-policy）。
- 主干定期扫描 fork，吸收高价值条目。

## 反面

- 不要绕过许可证。
- 不要删除 provenance。
- 不要让 fork 之间形成小圈子，脱离主干。

## 关联

[[A7-distribution-and-community]] [[patterns/distributed-evolution]] [[meta/pruning-policy]]