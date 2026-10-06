---
id: measure-hoisting-conflicts-before-workspace
type: pattern
status: draft
created: 2026-10-06
updated: 2026-10-06
aliases:
  - measure-hoisting-conflicts-before-workspace
enforced: null
author: heiniao
---

# 先量"提升冲突面"，再决定要不要 workspace

## 上下文

把"一个仓里的多个应用"变成真正的 workspace（npm / yarn / pnpm workspaces）时，
最容易只看到收益（共享代码、统一安装、原子提交），而忽略它的**前提**：
workspace 会把依赖**提升（hoist）**到根部——于是"各模块能不能共用一份依赖树"成了硬门槛。

## 问题

本仓 2026-10-06 实测（七个模块的关键依赖对照）：

- **三个 Next 应用分属三个版本**：`^16.2.10` / `^14.2.35` / `14.2.5` ✗ —— 单一 hoisted `next` 不可能同时满足。
- **三个 RN/Expo 应用不在同一条线上**：`19.0.0 + react-native 0.79.5 + expo 53` 与 `19.1.0 + 0.81.5 + expo ^54` ✗。
- 7 个模块**各有自己的 lockfile**；只有 2 个装了依赖。
- ⇒ **"先纳入纯 JS/Next 的子集"这个常见折中在本仓不成立** ✗：那三个应用本身就冲突。

框架级依赖（`next` / `expo` / `react-native`）与其他依赖不同：它们**带 CLI 与构建器**，
"靠 npm 自动嵌套版本救回来"很脆 —— 构建器的解析路径与运行期不一致时会以**难查的方式**失败。

## 方案

1. **先量冲突面**：把各模块的**框架级依赖**列成一张表（`next` / `react` / `react-native` / `expo` / `typescript`），
   逐列看有没有"同一个包出现多个不兼容版本"。**出现 ≥2 个不兼容版本 → 不要用会 hoist 的 workspace。**
2. **分支判据**：
   - 有冲突 + 仍想要 workspace → 选**隔离式**方案（pnpm 默认不 hoist ✓），并且**分期迁移**（一模块一次、每模块迁完跑门禁）；
   - 有冲突 + 不急 → **先不引入 workspace** ✓（见下条，大部分收益不依赖它）。
3. **不引入 workspace 也能拿到主要收益**：唯一包名 + 在 manifest 里**显式声明**共享包（`file:`）+ 根只做编排脚本 + 分层 hooks/CI。
   本仓即此形态 ✓：4 个消费方声明 `"@enatega/domain": "file:../packages/domain"`，
   根有 `gates:web` / `kb:validate` 编排，`.githooks` 分层挂钩 ✓ —— **依赖图已可解释，只是不共享安装**。
4. **判定"该上 workspace"的时机**：不是"看起来该有"，而是**重复安装/借工具开始真的疼**。
   本仓的真实痛点：`enatega-multivendor-api` 没有本地 `typescript` ✗ → 类型检查必须借 web 的编译器
   （且直接 `npx tsc` 会**静默地没跑起来**：exit 1 但 0 条 `error TS` ✗）。**这类痛点才值得动 lockfile。**

## 反面

- **为"整齐"动 N 份 lockfile** ✗ —— 迁移成本与回归面换来的只是形式统一。
- **只看共享代码，不看框架版本** ✗ —— 冲突恰恰出在框架级依赖上。
- **假设"纯 JS/Next 的子集一定安全"** ✗ —— 本仓实测：那三个 Next 应用互相冲突。
- **把 workspace 当成"共享包能被解析"的前提** ✗ —— 用 `file:` 声明同样能让依赖图说清（本仓已证）。

## 关联

[[ROOT]] [[patterns/module-identity-before-layout]] [[skills/S10-collab-cli]] [[meta/pruning-policy]]