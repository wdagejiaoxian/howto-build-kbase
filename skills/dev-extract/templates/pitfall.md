# ⚠️ 踩坑记录模板

## Frontmatter

```yaml
type: dev-note
subtype: pitfall
tech_stack: [技术栈列表]
severity: warning|caution|info
encounter_count: 1
last_encountered: YYYY-MM-DD
status: active
valid_versions: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
```

## 正文模板

````markdown
# [陷阱简述]

## 场景

[在做什么的时候遇到的]

## 陷阱描述

[具体是什么样的坑——你以为会怎样，实际是怎样]

## 原因

[为什么会这样——框架/语言的设计缺陷或隐式约定]

## 正确做法

```
# ❌ 错误写法
# ✅ 正确写法
```

## 适用版本

- 框架版本: x.x
- 注意: 在 x.x 中此问题已修复/仍存在

## 相关链接

> 根据配置中的 `link_style` 选择以下一种格式，删除其余两种：

**link_style = wiki**（双向链接）：
- [[concepts/相关概念]] - 相关概念说明

**link_style = tag**（标签格式）：
- #concept/相关概念 - 相关概念说明

**link_style = text**（纯文本）：
- 相关概念: [概念名] - 相关概念说明
````
