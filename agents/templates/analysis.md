> **渐进式加载**：本文件为按需加载层。仅在需要创建分析页（问答存档）时，由核心 AGENTS.md 或 CLAUDE.md 指示读取。

---

## 分析页模板

路径: `wiki/analysis/[主题].md`

```markdown
---
type: analysis
tags: []
created: YYYY-MM-DD
related_questions: []
sources: []
---

# [问题主题]

## 问题

[用户原始问题]

## 答案

[LLM基于Wiki生成的完整答案]

## 相关页面

- [[concepts/领域/概念名|概念名]]
- [[entities/类型/实体名|实体名]]

## 扩展思考

[可能的后续问题和探索方向]

## 更新日志

- YYYY-MM-DD: 首次创建
```
