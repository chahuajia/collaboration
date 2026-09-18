---
id: domain-purity-is-structural
type: pattern
status: active
source: 水利工程（分水岭）+ 洋葱架构
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [domain-purity-is-structural]
trigger: 领域层能不能碰框架；或差点把 Spring/JPA 注解标进 domain
enforced: evolutionary:backend/src/test/java/com/evolutionary/architecture/DomainFrameworkFreeTest.java
---

# 领域纯洁性是结构性必然，不是道德要求

## 上下文

反复强调"保持领域层纯洁"，但常被理解为"自律"或"纪律"。

## 问题

- **说法**："领域层不能 import 任何外部库" —— 听起来像"道德规范"。
- **后果**：有人说"我这次就 import 一下，方便" —— 破例。
- **真相**：**领域层的纯洁性不是"选择"，是"结构"** —— 一旦破坏，整个依赖图崩塌。

## 方案

### 结构性原因

**依赖方向的物理约束**：

- **所有外层依赖内层** —— 这是架构的"地基"。
- **领域层是最内层** —— 它不依赖任何东西。
- **一旦领域层 import 第三方** —— **它就有了依赖** —— **它不再是"最内层"**。

**后果链**：
领域层 import zod  
↓  
领域层的"最内层"身份被破坏  
↓  
应用层 import 领域层时，间接 import zod  
↓  
适配器 import 应用层时，间接 import zod  
↓  
任何"想换掉 zod"的尝试 —— 都要改整个系统  
↓  
架构的"可替换性"瓦解


### 为什么"侵蚀整个系统"

**不是"领域层变得不纯洁"** —— 是**"依赖图被改写"**。

**改写后的依赖图**：

- **原来**：外层 → 内层（单向）
- **破坏后**：内层 → 第三方 → 外层可能 → 内层（环）

**环的出现** —— **架构退化为"大泥球"**。

### 判据

**不是"这条规矩严不严"** —— 是**"破坏后依赖方向还在吗？"**

**检查方式**：

```powershell
# 检查领域层是否 import 了外部库
Select-String -Path "src/domain/**/*.ts" -Pattern "from\s+['\"](?!\.\.?/|@/)"
**结果为空** —— 领域纯洁。  
**结果非空** —— 依赖图已改写。

```
### 与其他原则的关系

- **A18 内外有别** —— 领域纯洁是"内层是内层"的前提。
    
- **S12 边界解析** —— 边界层是"过滤外部输入"的地方，领域层接收"已经干净的"输入。
## 反面

- 不要说"这次就破例一下" —— 破例就是改写依赖图。
    
- 不要以为"反正没人看" —— 架构是"结构性的"，不是"道德性的"。
    
- 不要把领域纯洁当作"教条" —— 它是"依赖方向的物理约束"。
    

## 关联

[[S7-ddd-frontend-light]] [[S12-边界解析]] [[patterns/what-how-are-projections]]