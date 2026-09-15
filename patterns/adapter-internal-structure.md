---
id: adapter-internal-structure
type: pattern
status: active
source: 六边形架构（Alistair Cockburn）
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [adapter-internal-structure]
---
# 适配器内部结构自由
## 上下文
需要组织"命令层"和"CLI 工具"的位置。
## 问题
- 把 `commands/` 和 `lib/` 放在 `src/` 顶层——它们无法归入任何架构层。
- 未来接入 HTTP API 时，`commands/` 名字被占用。
## 方案
**每个适配器有自己的一亩三分地**。
```text
src/
├── application/       ← 用例层（层）
├── domain/            ← 领域核心（层）
├── infrastructure/    ← 共享技术设施（层）
├── shared/            ← 零依赖工具（层）
├── cli/               ← 适配器 1（含 commands/ 和 lib/）
└── http/              ← 适配器 2（未来）

```
**适配器内部结构不影响领域和应用层**——所以可以自由组织。

### 依赖方向
```
       domain
         ↑
    application
     ↗       ↖
  cli     infrastructure
```

**适配器和基础设施可以互相依赖**——它们都是外层。

## 反面

- 不要把适配器的内部结构提升为顶层目录。
    
- 不要让领域/应用层依赖适配器。
## 关联

[[W3-ddd-refactor]] [[S7-ddd-frontend-light]] [[patterns/feature-discovery-over-hardcoded-paths]]
