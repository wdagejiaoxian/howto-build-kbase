# 知识库构建器

> 个人/团队知识库生成工具。5 分钟从零搭建结构化知识库。

---

## ✨ 特性

| 特性 | 说明 |
|------|------|
| **问卷引导** | 8 题问卷了解你的领域，自动生成分类目录 |
| **AI 编译** | 原始资料 → 摘要 → 概念 → 实体，全自动结构化 |
| **双向链接** | 知识页面自动互链，Obsidian 图形视图可视化 |
| **增量更新** | 只处理新增资料，不重复编译 |
| **健康检查** | Lint 检查矛盾、孤立页面、过时内容 |
| **开发知识点** | dev-extract 自动从对话中提炼 bug/配置/决策等 7 类知识 |
| **跨平台** | 支持 OpenCode、Claude Code 等 AI Agent 平台 |
| **渐进式加载** | 工作流和模板按需读取，减少 Token 消耗 |

---

## 🚀 快速开始

**5 分钟搭建你的第一个知识库**

### 第 1 步：克隆项目

```bash
git clone <仓库地址>
cd 知识库构建器
```

### 第 2 步：配置 Skill

将 `skills/` 目录下的两个 Skill 加载到你的 Agent 工具中：

**OpenCode**：
```bash
# 项目级加载
mkdir -p .opencode/skills
cp -r skills/knowledge-base-builder .opencode/skills/
cp -r skills/dev-extract .opencode/skills/
```

**Claude Code**：
```bash
# 安装为原生 Skill
mkdir -p .claude/skills/knowledge-base-builder
mkdir -p .claude/skills/dev-extract
cp skills/knowledge-base-builder/SKILL.md .claude/skills/knowledge-base-builder/
cp skills/dev-extract/SKILL.md .claude/skills/dev-extract/
```

### 第 3 步：初始化知识库

```
请帮我初始化知识库
```

AI 通过 8 题问卷了解你的领域和需求，自动生成分类目录。

### 第 4 步：投放资料

将 `.md`、`.txt`、`.pdf`、`.docx` 文件放入 `原始资料/` 目录。

### 第 5 步：AI 自动编译

```
请处理 原始资料/ 下的新文件
```

AI 自动生成摘要 → 提取概念和实体 → 建立双向链接。

### 第 6 步：日常使用

| 操作 | 命令 |
|------|------|
| 健康检查 | `请运行 lint 健康检查` |
| 问答查询 | `请查询知识库：XXX` |
| 更新内容 | `请更新 wiki/ 中的 XXX 页面` |
| 归档旧内容 | `请归档过时的 XXX` |

---

## 🎯 核心概念

### 三层架构

```
原始资料/  →  AI 自动编译  →  wiki/
  ↓              ↓              ↓
你放资料     生成摘要         结构化知识
             提取概念         双向链接
             提取实体
```

| 目录 | 说明 |
|------|------|
| `原始资料/` | 你放原始文件的地方（不可修改） |
| `wiki/` | AI 自动生成的结构化知识库 |

### 四类知识页面

| 类型 | 说明 |
|------|------|
| **summaries/** | 源文档摘要 |
| **concepts/** | 概念页（原理、方法论） |
| **entities/** | 实体页（项目、工具、人物） |
| **dev-notes/** | 开发知识点（bug、配置、决策等 7 类） |

---

## 📁 目录结构

```
知识库构建器/
├── AGENTS.md                    # AI 工作流规范（OpenCode 入口）
├── CLAUDE.md                    # AI 工作流规范（Claude Code 入口，内容同 AGENTS.md）
├── 教程.md                      # 完整使用教程（本地保留，不上传 Git）
├── OpenCode安装与配置.md         # OpenCode 安装与 Skill 配置指南
├── ClaudeCode安装与配置.md        # Claude Code 安装与 Skill 配置指南
├── agents/                      # 按需加载层（工作流和模板）
│   ├── templates/               # 页面模板
│   │   ├── entity.md
│   │   ├── concept.md
│   │   ├── summary.md
│   │   └── analysis.md
│   └── workflows/               # 工作流定义
│       ├── ingest.md            # 处理新源文档
│       ├── query.md             # 问答查询
│       ├── lint.md              # 健康检查
│       ├── update.md            # 更新已有页面
│       ├── archive.md           # 归档过时内容
│       └── extract.md           # 提炼开发知识点
├── skills/                      # Skill 集合
│   ├── knowledge-base-builder/  # 知识库构建 Skill
│   │   ├── SKILL.md
│   │   └── templates/
│   └── dev-extract/             # 开发知识点提炼 Skill
│       ├── SKILL.md
│       ├── .dev-extract.json.template
│       └── templates/
├── 原始资料/                    # 用户放置原始资料（不可修改）
└── wiki/                        # AI 自动生成的知识库
    ├── .ingest-state.md
    ├── .lint-state.md
    ├── index.md
    ├── log.md
    ├── entities/
    ├── concepts/
    ├── summaries/
    ├── analysis/
    └── dev-notes/
        ├── bugs/
        ├── pitfalls/
        ├── recipes/
        ├── decisions/
        ├── designs/
        ├── libs/
        └── perf/
```

---

## 🎯 解决了什么问题

**把知识当代码写，让 AI 当你的"编译器"。**

| 痛点 | 知识库构建器的解决方案 |
|------|----------------------|
| 手动整理笔记、分类、打标签，累 | **AI 自动编译**：把资料丢进去，AI 完成所有整理 |
| 资料散落在书签、微信收藏、文件夹 | **统一入口**：所有资料放入 `原始资料/`，AI 自动分类 |
| 搜索只能匹配关键词，找不到深层关联 | **双向链接 + 图形视图**：概念自动互链，Obsidian 可视化知识网络 |
| 知识过时无人维护，误导新人 | **健康检查**：Lint 自动检测矛盾、孤立页面、过时内容 |

---

## 📓 Obsidian 集成

知识库构建器与 **Obsidian** 完美配合，提供双向链接和图形视图。

| 特性 | 说明 |
|------|------|
| **双向链接** | `[[概念名]]` 自动互链，点击跳转 |
| **图形视图** | 知识网络可视化，发现隐藏关联 |
| **本地优先** | 所有数据存在本地，隐私安全 |

---

## 🛠️ 独立 Skill：dev-extract

`dev-extract` 可以**单独下载使用**。适合开发者在日常编码中自动提炼知识点。

### 单独使用方式

1. 下载 `skills/dev-extract/` 目录（含 `SKILL.md` + `templates/`）
2. 配置到你的 AI Agent 工具中
3. 开发对话中说"记录一下"即可触发

### 提炼的 7 类知识

| 类型 | 说明 |
|------|------|
| 🐛 报错与修复 | 报错信息 + 根因分析 + 修复步骤 |
| ⚠️ 踩坑记录 | 陷阱场景 + 错误写法 vs 正确写法 |
| 📋 配置配方 | 配置步骤 + 验证方法 + 参数说明 |
| 🔀 方案决策 | 方案对比 + 决策矩阵 + 原理深挖 |
| 🏗️ 架构设计 | 架构图 + 模块划分 + 数据流 |
| 📦 库/框架选型 | 候选对比 + 选择理由 + 使用心得 |
| ⚡ 性能优化 | 瓶颈定位 + 优化方案 + 效果对比 |

### 双模式

| 模式 | 说明 | 适用场景 |
|------|------|---------|
| **独立运行** | 知识点存在项目本地 | 项目独立沉淀 |
| **知识库联动** | 知识点汇入统一知识库 | 多项目共用知识库 |

> 首次使用时 AI 会通过 3 题问卷自动完成配置，无需手动编辑 JSON。

---

## 📓 Obsidian 集成

知识库构建器与 **Obsidian** 完美配合，提供最佳的知识浏览和管理体验。

### 为什么推荐 Obsidian？

| 特性 | 说明 |
|------|------|
| **双向链接** | `[[概念名]]` 自动互链，点击跳转 |
| **图形视图** | 知识网络可视化，发现隐藏关联 |
| **本地优先** | 所有数据存在本地，隐私安全 |
| **插件生态** | Dataview 查询、Templater 自动化、Excalidraw 画图 |
| **移动端** | Obsidian Mobile 随时随地查看 |

### 使用方式

1. 下载并安装 [Obsidian](https://obsidian.md/)
2. 打开知识库所在文件夹作为 Vault
3. 浏览 `wiki/` 目录下的知识页面
4. 点击 `[[链接]]` 在页面间跳转
5. 打开图形视图查看知识网络

### 推荐插件

| 插件 | 用途 |
|------|------|
| **Dataview** | 动态查询和统计知识页面 |
| **Templater** | 自动添加 frontmatter 和模板 |
| **Excalidraw** | 在知识页面中绘制架构图 |
| **Marp** | 将知识页面转为幻灯片 |

---

## 👥 适用人群

| 使用场景 | 说明 |
|---------|------|
| **个人知识整理** | 将散落的资料变成可查询的知识库 |
| **团队知识共享** | 统一格式，多人协作，Git 管理版本 |
| **开发知识沉淀** | dev-extract 自动从对话中提炼 bug/配置/决策等知识点 |
| **学习笔记管理** | AI 自动整理概念，建立知识关联 |

---

## 📄 许可证

本项目采用 MIT 许可证。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request。
