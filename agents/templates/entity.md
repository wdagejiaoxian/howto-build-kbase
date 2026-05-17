> **渐进式加载**：本文件为按需加载层。仅在需要创建或更新实体页时，由核心 AGENTS.md 或 CLAUDE.md 指示读取。

---

## 实体页模板

路径: `wiki/entities/[实体类型]/[实体名].md`

```markdown
---
type: entity
tags: [实体类型]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# [实体名]

## 基本信息

| 属性 | 值 |
|------|-----|
| 类型 | [人物/地点/项目/公司/网站/工具] |
| 首次记录 | YYYY-MM-DD |
| 最后更新 | YYYY-MM-DD |

## 核心信息

[实体的关键要点 - 2-3句话]

## 详细描述

[详细的背景、用途、特点]

## 相关源文档

- [[summaries/源文档名|源文档名]] - [一句话说明]

## 相关概念

- [[concepts/概念名]] - [关联说明]

## 相关实体

- [[entities/其他实体名]] - [关联说明]

## 更新日志

- YYYY-MM-DD: 首次创建
```
