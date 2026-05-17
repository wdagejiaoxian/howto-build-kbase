# ⚡ 性能优化模板

> ⚠️ 必须达到 L3 原理层

## Frontmatter

```yaml
type: dev-note
subtype: performance
tech_stack: [技术栈列表]
optimization_type: [SQL优化|缓存|算法|架构|配置]
encounter_count: 1
last_encountered: YYYY-MM-DD
created: YYYY-MM-DD
updated: YYYY-MM-DD
```

## 正文模板

````markdown
# [优化主题]

## 问题发现

[怎么发现性能问题的]

## 性能基线（优化前）

| 指标     | 值     |
| -------- | ------ |
| 响应时间 | 3000ms |
| QPS      | 50     |

## 瓶颈定位

### 定位过程

1. [用了什么工具/方法] → [发现了什么]

### 根因

[为什么会慢——底层原理]

## 优化方案

### 方案描述

[做了什么优化]

```sql
-- 优化前
-- 优化后
```

### 原理说明

[为什么这个优化有效——底层原理]

## 优化后结果

| 指标     | 优化前 | 优化后 | 提升 |
| -------- | ------ | ------ | ---- |
| 响应时间 | 3000ms | 50ms   | 60x  |

## 注意事项

- ⚠️ [副作用]
- ⚠️ [什么情况下优化会失效]

## 相关链接

> 根据配置中的 `link_style` 选择以下一种格式，删除其余两种：

**link_style = wiki**（双向链接）：
- [[concepts/相关概念]] - 相关概念说明
- [[dev-notes/perf/相关优化]] - 关联的优化知识

**link_style = tag**（标签格式）：
- #concept/相关概念 - 相关概念说明
- #dev-note/相关优化 - 关联的优化知识

**link_style = text**（纯文本）：
- 相关概念: [概念名] - 相关概念说明
- 关联优化: [优化名] - 关联的优化知识
````
