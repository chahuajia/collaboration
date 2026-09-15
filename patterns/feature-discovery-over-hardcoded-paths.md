---
id: feature-discovery-over-hardcoded-paths
type: pattern
status: active
source: 航海导航（星辰定位）
created: 2026-09-15
updated: 2026-09-15
author: heiniao
aliases: [feature-discovery-over-hardcoded-paths]
---

# 特征发现优于硬编码路径

## 上下文

需要在目录树中定位（项目根、git 仓库、配置目录）。

## 问题

- `path.resolve(__dirname, '../../..')` 依赖"层级数"。
- 目录一移动，路径就崩。
- 跨平台行为不一致。

## 方案

**用"标志物"定位，不用"相对层级"**。

| 目标 | ❌ 硬编码 | ✅ 特征发现 |
| :--- | :--- | :--- |
| 项目根 | `../../..` | 向上找 `package.json` |
| git 仓库 | `path.join(__dirname, '../repo')` | 向上找 `.git` |
| COLLABORATION | 硬编码路径 | 向上找 `COLLABORATION/` |
| tsconfig | `./tsconfig.json` | 向上找 `tsconfig.json` |

### 骨架

```ts
function findUp(startDir: string, marker: string): string | null {
  let dir = path.resolve(startDir);
  while (true) {
    if (fs.existsSync(path.join(dir, marker))) return dir;
    const parent = path.dirname(dir);
    if (parent === dir) return null;
    dir = parent;
  }
}
```

## 反面

- 不要假设"从 A 到 B 有几层"。
    
- 不要在测试里硬编码 `../../..`。
    
- 不要用"位置"作为"身份"。
    

## 关联

[[S29-批替换决策]] [[S30-批处理脚本骨架]] [[patterns/structure-over-algorithm]]
