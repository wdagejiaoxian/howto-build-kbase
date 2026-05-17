# 📋 配置配方模板

## Frontmatter

```yaml
type: dev-note
subtype: recipe
tech_stack: [技术栈列表]
env: [开发环境|生产环境|测试环境]
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
# [配置目标]

## 适用场景

[什么情况下需要这个配置]

## 前置条件

- [ ] 已安装 xxx
- [ ] 已配置 xxx

## 配置步骤

### 1. [步骤1标题]

```yaml
# 配置示例
```

### 2. [步骤2标题]

```bash
# 执行命令
```

## 验证方法

```bash
# 验证配置是否生效
```

## 常见问题

- Q: xxx 报错？
  A: 检查 xxx 配置项

## 关键参数说明

- `参数A`: [含义和取值原因]
- `参数B`: [为什么设这个值]

## 相关链接

> 根据配置中的 `link_style` 选择以下一种格式，删除其余两种：

**link_style = wiki**（双向链接）：
- [[concepts/相关概念]] - 相关概念说明
- [[dev-notes/recipes/相关配置]] - 相关配置

**link_style = tag**（标签格式）：
- #concept/相关概念 - 相关概念说明
- #dev-note/相关配置 - 相关配置

**link_style = text**（纯文本）：
- 相关概念: [概念名] - 相关概念说明
- 相关配置: [配置名] - 相关配置
````
