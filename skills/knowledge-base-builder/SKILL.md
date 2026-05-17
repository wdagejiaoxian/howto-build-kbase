---
name: knowledge-base-builder
description: 引导用户初始化专属知识库结构。通过 8 题问卷了解用户背景，动态生成分类目录，并配置 AGENTS.md 和 agents/ 工作流。适用于任何领域（技术/学术/业务/学习）。
---

# 知识库构建器

引导用户从零初始化一个结构化的个人/团队知识库。通过问卷了解用户背景，动态生成分类目录，配置工作流。

**渐进式加载**：本文件为核心层（始终加载），模板按需从 `templates/` 目录读取。

---

## 触发条件

- 用户首次使用知识库构建器
- 用户说"初始化知识库"、"开始构建"、"setup"、"configure" 等关键词
- 用户希望调整现有知识库的分类结构

---

## 核心工作流

```
Step 1: 欢迎与介绍 → 说明知识库构建器是什么
Step 2: 发起问卷 → 读取 templates/questionnaire.md，一次性抛出问题
Step 3: 分析问卷结果 → 匹配 templates/domain-templates/ 中的模板
Step 4: 创建目录结构 → 原始资料/ 分类 + wiki/ 初始化
Step 5: 配置 AGENTS.md → 更新领域分类表
Step 6: 引导首次使用 → 指导投放资料 + 演示 Ingest
```

---

## 问卷设计

**加载**: `templates/questionnaire.md`

8 题问卷（Q1-Q8），覆盖身份、领域、格式、数量、分类、检索习惯、核心需求、个性化需求。用户至少回答 Q1-Q3 即可开始。

---

## 领域模板映射

**加载**: `templates/domain-templates/[匹配模板].md`

| 模板 | 适用身份 | 模板文件 |
|------|---------|---------|
| **技术开发者** | 程序员、架构师、DevOps | `domain-templates/developer.md` |
| **学术研究者** | 研究生、教授、研究员 | `domain-templates/researcher.md` |
| **业务管理者** | 产品经理、运营、市场 | `domain-templates/manager.md` |
| **通用学习者** | 学生、自学者、爱好者 | `domain-templates/learner.md` |

不匹配任何模板时，根据问卷结果**动态生成**分类。

---

## 资料自动分类

当用户将资料放入 `原始资料/` 根目录 或者 `原始资料/待分类/` 时触发。

**加载**: `templates/reports/classification-report.md`

5 步流程：扫描 → 分析 → 判定（>80% 自动/50-80% 待确认/<50% 无法分类）→ 报告 → 移动。

用户可调整：确认/修改单个/新建分类/合并分类。

---

## 配置项

可在项目根目录创建 `.knowledge-base.json`：

```json
{
  "domain": "",
  "categories": [],
  "wiki_structure": {
    "concepts_dir": "concepts",
    "entities_dir": "entities",
    "summaries_dir": "summaries",
    "analysis_dir": "analysis",
    "dev_notes_dir": "dev-notes"
  },
  "link_style": "wiki",
  "language": "zh-CN"
}
```

| 字段 | 默认值 | 说明 |
|------|--------|------|
| `domain` | `""` | 用户领域名称 |
| `categories` | `[]` | 自定义分类列表 |
| `wiki_structure` | 见上 | wiki 子目录名称 |
| `link_style` | `"wiki"` | `wiki`/`tag`/`text` |
| `language` | `"zh-CN"` | 知识库语言 |

---

## 目录创建规则

### 原始资料/

```
原始资料/
├── [分类1]/
├── [分类2]/
├── [分类3]/
└── 待分类/
```

### wiki/

```
wiki/
├── entities/{projects,tools,platforms}/
├── concepts/[领域]/
├── summaries/
├── analysis/
├── dev-notes/{bugs,pitfalls,recipes,decisions,designs,libs,perf}/
├── .ingest-state.md  # 使用 templates/wiki-initial/ingest-state.md
├── .lint-state.md    # 使用 templates/wiki-initial/lint-state.md
├── index.md          # 使用 templates/wiki-initial/index.md
└── log.md            # 使用 templates/wiki-initial/log.md
```

---

## 工作流触发器

本 Skill 不重新定义工作流细节，而是作为 **AGENTS.md 和 agents/ 目录工作流的触发器和调度器**。所有工作流的具体执行步骤、页面模板、格式要求均以 `AGENTS.md` 和 `agents/` 目录中的定义为准。

### 触发器索引

| 工作流 | 触发关键词 | 报告模板 | 工作流文件 |
|--------|-----------|---------|-----------|
| **Ingest** | "处理资料"、"开始 Ingest" | `templates/reports/ingest-report.md` | `agents/workflows/ingest.md` |
| **Query** | "查询"、"请问"、"什么是" | — | `agents/workflows/query.md` |
| **Lint** | "检查知识库"、"运行 Lint" | `templates/reports/lint-report.md` | `agents/workflows/lint.md` |
| **Update** | "更新 [页面]"、"修正 [概念]" | — | `agents/workflows/update.md` |
| **Archive** | "归档 [页面]"、"清理过时内容" | — | `agents/workflows/archive.md` |
| **Extract** | "记录一下"、"提炼"、"extract" | — | `skills/dev-extract/SKILL.md` |

### Ingest 工作流触发

**触发条件**: 用户说"处理资料"、"开始 Ingest"、"编译知识"、投放新资料后请求处理

**执行**:
1. 读取 `agents/workflows/ingest.md` 获取 Ingest 工作流定义
2. **严格**按该文件流程执行
3. 输出处理报告（使用 `templates/reports/ingest-report.md`）

**增量更新**: 读取 `wiki/log.md` 对比已处理文件，只处理新增文件。

**错误处理**: 格式不支持→跳过；文件为空→跳过；领域无法判定→询问用户；页面生成失败→记录错误继续。

### Query 工作流触发

**触发条件**: 用户提问关键词（"查询"、"请问"、"什么是"、"如何使用"）

**执行**:
1. 读取 `agents/workflows/query.md` 获取 Query 工作流定义
2. 按该文件流程执行
3. 答案必须标注引用来源：`[[wiki/路径|显示名]]`

**存档评估**: 核心概念/可复用代码/常见疑问/横向对比 → 有价值时提示用户是否存档。

**无结果处理**: 提示用户投放资料或基于通用知识回答。

### Lint 工作流触发

**触发条件**: "检查知识库"、"运行 Lint"、"健康检查"、"每周维护"

**执行**:
1. 读取 `agents/workflows/lint.md` 获取 Lint 工作流定义
2. **严格**按该文件流程执行（7 步检查）
3. 输出 Lint 报告（使用 `templates/reports/lint-report.md`）

**自动修复**（无需确认）: 失效链接→删除；格式错误→补全 frontmatter；日期未更新→修正。

**需用户确认**: 矛盾信息/孤立页面/重复摘要/归档建议。

### Update 工作流触发

**触发条件**: "更新 [页面名]"、"修正 [概念] 的内容"、源文档更新

**执行**:
1. 读取 `agents/workflows/update.md` 获取 Update 工作流定义
2. 按该文件流程执行
3. 确认更新完成

### Archive 工作流触发

**触发条件**: "归档 [页面名]"、"清理过时内容"、Lint 建议归档、超过 6 个月未更新

**执行**:
1. 读取 `agents/workflows/archive.md` 获取 Archive 工作流定义
2. 按该文件流程执行
3. 归档规则：状态改为 `archived`，保留原内容，index.md 移入「已归档」分区

### Extract 工作流触发

**触发条件**: "记录一下"、"提炼"、"extract"、"存档"

**执行**:
1. 加载 `skills/dev-extract/SKILL.md`
2. 按 dev-extract 流程执行
3. 支持双模式：知识库联动（linked）或独立运行（standalone）

---

## 分类调整能力

用户可在使用过程中调整分类结构（新增、删除、合并、重命名）。

### 触发条件

- "新增一个 XX 分类"、"把 XX 和 XX 合并"、"删除 XX 分类"
- "重命名 XX 为 YY"
- Lint 检查后建议调整分类

### 调整类型

| 类型 | 操作概要 |
|------|---------|
| **新增** | 创建目录 → 更新 AGENTS.md 领域表 → 确认 |
| **删除** | 检查文件 → 处理文件（移动/删除）→ 更新 AGENTS.md → 确认 |
| **合并** | 移动文件 → 删除空目录 → 更新 AGENTS.md + index.md → 确认 |
| **重命名** | 重命名目录 → 更新 AGENTS.md + log.md + wiki 链接 → 确认 |

### 影响范围检查

| 检查项 | 操作 |
|--------|------|
| log.md 中的路径 | 更新所有引用旧分类路径的记录 |
| wiki 页面中的链接 | 更新 `[[summaries/...]]` 等链接 |
| AGENTS.md 领域表 | 添加/删除/修改对应行 |
| index.md 分类 | 更新索引中的分类结构 |

### 调整确认流程

```
Step 1: 用户提出调整请求
Step 2: AI 分析影响范围
Step 3: AI 输出调整计划（操作/影响/确认执行?）
Step 4: 用户确认 → 执行调整
Step 5: 输出调整结果报告
```

---

## 输出格式

初始化完成后输出：

```
## 知识库初始化完成 ✅

**领域**: [领域名称]
**分类结构**:
- 原始资料/[分类1]/
- 原始资料/[分类2]/
- ...

**Wiki 结构**:
- wiki/concepts/
- wiki/entities/
- wiki/summaries/
- ...

**下一步**:
1. 将你的资料放入 原始资料/ 对应分类
2. 说"请处理这些资料"开始 Ingest
3. 或说"查询 [问题]"开始使用知识库
```

---

## 注意事项

1. **不要猜测用户意图**：问卷回答模糊时，列出选项让用户选择
2. **分类不宜过多**：建议 3-7 个大类，避免过细
3. **保留扩展性**：用户后续可随时要求新增/合并分类
4. **语言一致**：全程使用用户选择的语言（默认中文）
5. **平台无关**：本 Skill 不依赖特定 Agent 平台
