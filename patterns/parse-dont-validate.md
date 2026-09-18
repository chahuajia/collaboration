---
id: parse-dont-validate
type: pattern
status: active
source: 函数式编程（Alexis King）
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [parse-dont-validate]
trigger: 边界上拿 raw/unknown 做校验；或领域层在解析字符串协议
enforced: null
---
# Parse, don't validate
## 上下文
需要把外部输入转换为内部类型。
## 问题
- 校验只返回布尔值，不产出类型化数据。
- 领域层被迫处理原始类型。
## 方案
- **边界负责解析**：接收 `unknown`，输出类型化的 `*Input`。
- **领域只接收类型化数据**。
- **边界层是唯一生产者**，领域层是唯一消费者。
- 类型校验（形状）在边界；业务不变量在领域。
## 反面
- 不要让领域层处理 `unknown`。
- 不要让边界层做业务校验。
- 不要让调用方手动构造 `*Input`。
## 关联
[[S12-边界解析]] [[S13-Smart-Constructor]] [[patterns/value-object-as-raw-material]]
