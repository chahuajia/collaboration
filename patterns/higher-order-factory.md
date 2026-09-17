---
id: higher-order-factory
type: pattern
status: active
source: 函数式编程（高阶函数）+ 框架设计
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [higher-order-factory]
trigger: 通用工厂只服务最简场景；或例外只能绕过工厂；或想让用户注入 before/after 却不知怎么切
---
# 高阶工厂
## 上下文
设计"通用工厂"（如测试的 `useTestWorkspace`）时，容易陷入"只服务最简场景"。
## 问题
- **第一版**：只服务 90% 场景（"创建后什么都不做"）。
- **僵化**：遇到例外（"创建后追加操作"）就得"绕过"。
- **演进**：从"库"到"框架" —— 让用户"注入"特定部分。
## 方案
### 一、三段划分
| 段 | 角色 | 谁写 |
| :--- | :--- | :--- |
| **通用 How** | 框架管 | 工厂 |
| **特定 What** | 用户注入 | 回调 |
| **两者独立演化** | 各自改 | — |
### 二、实现

```ts
export function useTestWorkspace(opts: {
  before?: (ctx: WorkspaceContext) => Promise<void>;
  after?: (ctx: WorkspaceContext) => Promise<void>;
} = {}): () => WorkspaceContext {
  let current: WorkspaceContext;
  beforeEach(async () => {
    current = await createWorkspace(opts);
    if (opts.before) await opts.before(current);   // 注入
  });
  afterEach(async () => {
    if (current) {
      if (opts.after) await opts.after(current);   // 注入
      await cleanupWorkspace(current.root);
    }
  });
  return () => current;
}

```
### 三、"库" vs "框架"

|类型|特征|例子|
|---|---|---|
|**库**|假设"你要做什么"|`createWorkspace()`|
|**框架**|不假设 —— 提供"钩子"|`useTestWorkspace({ before, after })`|

**"从库到框架" = "高阶化"**。

### 四、何时高阶化

**触发条件**：

- **出现第二处例外** —— 说明"通用"不够。
    
- **"绕过"的代码重复** —— 说明"缺钩子"。
    
- **用户想"复用"但"不适用"** —— 说明"该注入"。
    

## 反面

- 不要一开始就"最高阶" —— **先做最简版**。
    
- 不要在"只有一个场景"时高阶化 —— **过度设计**。
    
- 不要让钩子"无所不包" —— **钩子是"特定部分"，不是"所有事"**。
    

## 关联

[[S31-测试设计原则]] [[A10-review-前置原则]] [[patterns/derivation-over-copy]]
