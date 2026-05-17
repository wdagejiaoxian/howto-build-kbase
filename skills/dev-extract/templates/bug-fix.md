# 🐛 报错与修复模板

## Frontmatter

```yaml
type: dev-note
subtype: bug-fix
tech_stack: [技术栈列表]
severity: critical|major|minor
encounter_count: 1
last_encountered: YYYY-MM-DD
status: active
valid_versions: []
expires:
created: YYYY-MM-DD
updated: YYYY-MM-DD
```

## 正文模板

````markdown
# [错误简述]

## 错误现象

[完整的报错信息、日志片段]

## 复现条件

1. [步骤1]
2. [步骤2]
3. [触发]

## 根因分析

[为什么会出现这个错误——底层原因]

## 修复步骤

1. [具体操作]

```bash
# 可执行的修复命令
```

## 预防措施

- [如何避免再次遇到]

## 遇到记录

| 日期       | 环境/版本 | 备注   |
| ---------- | --------- | ------ |
| YYYY-MM-DD | [版本]    | [说明] |

## 排查思路（如果排查过程复杂）

1. [先检查了什么]
2. [排除了什么]
3. [最终定位方法]

## 相关链接

> 根据配置中的 `link_style` 选择以下一种格式，删除其余两种：

**link_style = wiki**（双向链接）：
- [[concepts/相关概念]] - 相关概念说明
- [[dev-notes/相关知识点]] - 关联的其他 bug

**link_style = tag**（标签格式）：
- #concept/相关概念 - 相关概念说明
- #dev-note/相关知识点 - 关联的其他 bug

**link_style = text**（纯文本）：
- 相关概念: [概念名] - 相关概念说明
- 关联知识点: [知识点名] - 关联的其他 bug
````
