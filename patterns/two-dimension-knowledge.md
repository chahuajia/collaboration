---
id: two-dimension-knowledge
type: pattern
status: draft
source: 城市规划（分区）+ 图书馆学（分面分类）
created: 2026-09-15
updated: 2026-09-17
author: heiniao
aliases: [two-dimension-knowledge]
trigger: 知识分类只做成临时→长期一维；或分不清个人/项目/组织知识该放哪；或想上二维却不知现在够不够一维
enforced: null
---

# 知识的二维空间

## 上下文

设计"知识分类系统"时，直觉上会做成一维（如"临时→中期→长期"）。

## 问题

- 一维模型会**丢失范围维度**——"个人"和"组织"的知识无法区分。
- 真实世界的知识**至少两维**：成熟度 × 范围。

## 方案

### 二维模型

|        | 临时             | 中期   | 长期            |
| :----- | :------------- | :--- | :------------ |
| **个人** | 草稿             | 私人笔记 | 个人知识库         |
| **项目** | working-memory | 项目笔记 | COLLABORATION |
| **组织** | 组织协作流          | 组织文档 | 组织规范          |

### 当前实施：只做一维

- **当前只有一个项目**（collab-cli）→ 不需要范围维度。
- **当前只有一个"组织"**（你）→ 不需要组织维度。
- **只实施"成熟度"一维**。

### 扩展点（记录，不实施）

**方式 A：加项目前缀**
```text
working-memory/
  ├── collab-cli-progress.md
  └── other-project-progress.md
  
```

**方式 B：加目录层级**
projects/
  ├── collab-cli/
  └── other-project/
**方式 C：加组织层级**
my-org/
  ├── org-level/
  └── projects/
**触发条件**：

- 有**第二个项目** → 升级到方式 A。
    
- 有**第二个协作者** → 需要更明确的项目/组织边界。
    

## 反面

- 不要在小镇时就规划地铁——过度设计。
    
- 不要假装"一维就是全部"——真实世界通常是多维的。
    
- 不要为"未来的扩展"过早优化目录结构。
    

## 关联

[[A14-多实体协作原则]] [[W10-working-memory]] [[patterns/faceted-classification]]
