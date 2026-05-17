# AGENTS.md - Wiki知识库工作规范

> 本文件为**核心层**（始终加载）。所有详细工作流和页面模板已拆分为独立文件，按需读取。
> 拆分文件位于 `agents/` 目录下，详见下方内容索引。

---

## 三层架构

| 层级 | 位置 | 说明 |
|------|------|------|
| **Raw Sources** | `原始资料/` | 不可修改，仅 LLM 读取 |
| **Wiki** | `wiki/` | LLM 生成的结构化知识 |
| **Schema** | 本文件 + `agents/` | 工作流程规范 |

## 目录结构

```
[项目根目录]/
├── 原始资料/              # Raw Sources（不可修改）
│   └── [用户自定义分类]/  # 领域分类由问卷动态生成
│
├── wiki/                  # Wiki（LLM生成）
│   ├── .ingest-state.md  # Ingest增量状态（分批执行跟踪）
│   ├── .lint-state.md    # Lint状态跟踪（增量检查用）
│   ├── index.md          # 知识库索引
│   ├── log.md            # 操作日志
│   ├── entities/         # 实体页
│   │   ├── projects/     # 项目实体
│   │   ├── tools/        # 工具实体
│   │   └── platforms/    # 平台实体
│   ├── concepts/         # 概念页（领域子目录由用户自定义）
│   │   └── [领域]/       # 领域分类由用户动态生成
│   ├── summaries/        # 源文档摘要
│   ├── analysis/         # 问答存档
│   └── dev-notes/        # 开发知识点（Extract工作流产出）
│       ├── bugs/        # 报错与修复
│       ├── pitfalls/    # 踩坑记录
│       ├── recipes/     # 配置配方
│       ├── decisions/   # 方案决策
│       ├── designs/     # 架构设计
│       ├── libs/        # 库/框架选型
│       ├── perf/        # 性能优化
│       ├── index.md     # 开发知识索引
│       └── README.md    # 分区说明
│
├── agents/                # 按需加载层（工作流和模板）
│   ├── templates/        # 页面模板
│   └── workflows/        # 工作流定义
│
├── AGENTS.md              # OpenCode 规则文件（核心层）
└── CLAUDE.md              # Claude Code 规则文件（内容与 AGENTS.md 一致）
```

> **领域分类说明**：`原始资料/` 和 `wiki/concepts/` 下的领域子目录由用户通过问卷动态生成，不预设固定分类。

---

## 核心规则

1. **不修改 Raw Sources** - 绝不编辑原始资料目录下的文件
2. **使用模板** - 所有新页面必须基于 `agents/templates/` 中的模板创建
3. **frontmatter 必填** - 所有页面必须包含完整 frontmatter

   ```yaml
   ---
   type: entity|concept|summary|dev-note
   tags: []
   created: YYYY-MM-DD
   updated: YYYY-MM-DD
   sources: []
   ---
   ```

   > dev-note 类型额外需要 `subtype`、`tech_stack`、`encounter_count`、`last_encountered`、`status` 字段。详见 `skills/dev-extract/SKILL.md`

4. **更新 log** - 每次 ingest/update/archive 必须记录到 `wiki/log.md`
5. **更新 index** - 每次 ingest 必须同步 `wiki/index.md`
6. **维护 lint-state** - ingest/update 必须将涉及页面追加到 `wiki/.lint-state.md` 的待检查列表；archive 必须移除对应页面
7. **维护 ingest-state** - ingest 必须将批次状态实时更新到 `wiki/.ingest-state.md`；每个文件的三步（摘要→概念→实体）必须按顺序完成
8. **内部链接** - 使用 `[[wiki/path|显示名]]` 格式；相对路径用 `../summaries/文件名.md`
9. **代码可执行** - 示例代码必须可用

---

## 内容索引

当需要执行对应操作时，加载索引中对应的子文件：

### 页面模板（agents/templates/）

| 操作 | 加载文件 | 说明 |
|------|---------|------|
| 创建/更新实体页 | `agents/templates/entity.md` | 人物/项目/工具等 |
| 创建/更新概念页 | `agents/templates/concept.md` | 技术概念/方法论 |
| 创建/更新摘要页 | `agents/templates/summary.md` | 源文档摘要 |
| 创建/更新分析页 | `agents/templates/analysis.md` | 问答存档 |

### 工作流（agents/workflows/）

| 操作 | 加载文件 | 触发场景 |
|------|---------|---------|
| Ingest | `agents/workflows/ingest.md` | 处理新源文档 |
| Query | `agents/workflows/query.md` | 问答查询 |
| Lint | `agents/workflows/lint.md` | 健康检查/每周维护 |
| Update | `agents/workflows/update.md` | 修正已有页面 |
| Archive | `agents/workflows/archive.md` | 清理过时内容 |
| Extract | `agents/workflows/extract.md` | 提炼开发知识点 |

---

## 命名规范

| 类型 | 路径格式 |
|------|----------|
| 概念页 | `wiki/concepts/[领域]/[概念名].md` |
| 实体页 | `wiki/entities/[实体类型]/[实体名].md` |
| 摘要页 | `wiki/summaries/[源文档名].md` |
| 分析页 | `wiki/analysis/[主题].md` |
| 开发知识点 | `wiki/dev-notes/{bugs,pitfalls,recipes,decisions,designs,libs,perf}/[YYYY-MM-DD-标题].md` |
