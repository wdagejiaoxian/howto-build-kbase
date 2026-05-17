---
name: dev-extract
description: 从开发对话中自动提炼知识点，存入可配置的 wiki 分区。支持双模式：知识库联动（linked）和独立运行（standalone）。首次使用 AI 引导配置（3 题问卷自动生成 .dev-extract.json），无需手动编辑。覆盖 7 类知识：报错修复、踩坑记录、配置配方、方案决策、架构设计、库/框架选型、性能优化。手动触发（记录一下/extract）或 session 结束自动扫描。采用渐进式加载，模板按需读取。
---

# 开发知识点自动提炼

从开发对话中提炼结构化的开发知识点，存入用户配置的 wiki 分区。

**渐进式加载**：本文件为核心层（始终加载），模板按需从 `templates/` 目录读取。

---

## 配置

### 首次使用：AI 引导配置

当用户首次触发 dev-extract 时，AI 检查 `.dev-extract.json` 是否存在：
- **存在** → 直接读取配置，执行提炼
- **不存在** → 发起 3 题问卷，自动生成配置文件

#### 3 题问卷（仅首次）

**Q1: 存储位置**

> 这些开发知识点，你希望存到哪里？
> 1. 就放本项目里（独立使用）
> 2. 存到我的统一知识库（多项目共用）
> 3. 不知道/随便

| 用户选择 | AI 推断配置 | 后续 |
|---------|------------|------|
| 1（独立） | `wiki_root=""`, `mode=standalone` | 直接进入 Q3 |
| 2（联动） | `mode=linked` | 进入 Q2 |
| 3（随便） | 推荐独立模式，直接跳过问卷 | 结束 |

**Q2: 知识库路径（仅选 2 的用户）**

AI 自动扫描相邻目录，列出候选知识库：

```
我检测到以下可能的知识库目录：
1. E:/obsidian存储仓库/知识库  （包含 wiki/ 目录）
2. D:/我的笔记/技术知识库      （包含 wiki/ 目录）

请选择编号，或手动输入完整路径。
```

用户选编号 → AI 自动填入 `wiki_root`。用户手动输入 → AI 验证路径有效性（目录存在且包含 `wiki/` 或 `dev-notes/`）。

**Q3: 链接方式**

> 你希望这些知识点能和知识库里的概念页互相跳转吗？
> 1. 当然，能互相跳转最好
> 2. 简单记录就行，不用链接

| 用户选择 | AI 操作 | 推断配置 |
|---------|--------|---------|
| 1（跳转） | 检查 `concepts_dir` 是否存在 → 存在则 `link_style=wiki`，不存在则 `link_style=text` 并告知原因 | `link_style=wiki` 或 `text` |
| 2（不用） | 不检查，直接使用纯文本 | `link_style=text` |

#### 生成配置

问卷结束后，AI 自动生成 `.dev-extract.json` 并展示给用户确认：

```json
{
  "wiki_root": "用户选择的路径或空字符串",
  "wiki_base": "wiki",
  "dev_notes_dir": "dev-notes",
  "mode": "linked 或 standalone",
  "concepts_dir": "concepts",
  "entities_dir": "entities",
  "link_style": "wiki 或 text",
  "categories": {
    "bugs": "bugs",
    "pitfalls": "pitfalls",
    "recipes": "recipes",
    "decisions": "decisions",
    "designs": "designs",
    "libs": "libs",
    "perf": "perf"
  },
  "index_file": "index.md",
  "log_file": "log.md"
}
```

用户确认 → 保存文件，开始提炼。

### 技术用户：手动配置

技术用户可直接创建 `.dev-extract.json` 跳过问卷：

```json
{
  "wiki_root": "",
  "wiki_base": "wiki",
  "dev_notes_dir": "dev-notes",
  "mode": "auto",
  "concepts_dir": "concepts",
  "entities_dir": "entities",
  "link_style": "auto",
  "categories": {
    "bugs": "bugs",
    "pitfalls": "pitfalls",
    "recipes": "recipes",
    "decisions": "decisions",
    "designs": "designs",
    "libs": "libs",
    "perf": "perf"
  },
  "index_file": "index.md",
  "log_file": "log.md"
}
```

| 字段 | 默认值 | 说明 |
|------|--------|------|
| `wiki_root` | `""`（空） | **全局 wiki 绝对路径**。非空时覆盖 `wiki_base`，所有项目写入同一知识库 |
| `wiki_base` | `wiki` | wiki 根目录名（相对于项目根目录，`wiki_root` 为空时生效） |
| `dev_notes_dir` | `dev-notes` | 开发知识点子目录名 |
| `mode` | `"auto"` | `linked`/`standalone`/`auto`。见下方模式判定逻辑 |
| `concepts_dir` | `"concepts"` | 概念页目录名（支持自定义如 `概念`） |
| `entities_dir` | `"entities"` | 实体页目录名（支持自定义如 `实体`） |
| `link_style` | `"auto"` | `wiki`/`tag`/`text`/`auto`。`wiki`=双向链接，`tag`=标签格式，`text`=纯文本 |
| `categories` | 见上 | 7 类知识的子目录名 |
| `index_file` | `index.md` | 索引文件名 |
| `log_file` | `log.md` | 日志文件名 |

> **提示**：非技术用户无需手动编辑此文件，AI 会通过 3 题问卷自动生成。技术用户可直接创建/修改。

### 修改配置

用户可随时说"修改配置"重新触发问卷，AI 会：
1. 读取现有 `.dev-extract.json`
2. 展示当前配置
3. 询问需要修改哪一项

**可修改项**：
| 修改项 | 用户说法示例 | AI 操作 |
|--------|-------------|--------|
| 存储位置 | "改成本项目独立使用" / "改成存到统一知识库" | 修改 `wiki_root` 和 `mode` |
| 知识库路径 | "知识库换到 D:/xxx" | 验证新路径，更新 `wiki_root` |
| 链接方式 | "改成双向链接" / "不用链接了" | 修改 `link_style` |
| 目录名称 | "概念页目录改成 `概念`" | 修改 `concepts_dir` 或 `entities_dir` |
| 知识分类 | "bugs 目录改成 `报错`" | 修改 `categories.bugs` |

4. 更新配置文件，确认生效

### 模式判定逻辑（`mode: "auto"`）

AI 在首次运行时自动执行以下判定：

```
Step 1: 读取 .dev-extract.json 配置
Step 2: 检查 wiki_root 是否为空
Step 3: 如果 wiki_root 非空 → 检查 concepts_dir 和 entities_dir 是否存在
Step 4: 根据检查结果判定 mode 和 link_style
Step 5: 输出判定结果告知用户
```

**判定决策表**：

| # | 情况 | wiki_root | concepts_dir | entities_dir | 判定结果 |
|---|------|-----------|--------------|--------------|---------|
| 1 | 完整联动 | 非空 | ✅ 存在 | ✅ 存在 | `mode=linked`, `link_style=wiki` |
| 2 | 目录名不同 | 非空 | ✅ 存在(自定义名) | ✅ 存在(自定义名) | `mode=linked`, `link_style=wiki` |
| 3 | 部分联动 | 非空 | ❌ 不存在 | ❌ 不存在 | `mode=linked`, `link_style=text` |
| 4 | 跨项目路径 | 非空(绝对路径) | ✅ 存在 | ✅ 存在 | `mode=linked`, `link_style=wiki` |
| 5 | 独立运行 | 空 | — | — | `mode=standalone`, `link_style=text` |

**用户手动覆盖**：
在配置文件中显式设置 `mode` 和 `link_style` 可跳过自动判定：
```json
{
  "mode": "linked",        // 强制联动模式
  "link_style": "wiki"     // 强制使用双向链接
}
```

### 路径解析

| 模式 | 知识页路径 | 索引/日志路径 |
|------|-----------|--------------|
| `wiki_root` 非空 | `{wiki_root}/{dev_notes_dir}/{category}/{YYYY-MM-DD-标题}.md` | `{wiki_root}/{index_file}`、`{wiki_root}/{log_file}` |
| `wiki_root` 为空 | `{wiki_base}/{dev_notes_dir}/{category}/{YYYY-MM-DD-标题}.md` | `{wiki_base}/{index_file}`、`{wiki_base}/{log_file}` |

未找到配置文件时，使用默认值并提示用户。

---

## 链接适配策略

根据 `mode` 和 `link_style` 动态生成 concepts/entities 链接：

| 情况 | 条件 | 链接格式 |
|------|------|---------|
| **完整联动** | `wiki_root` 非空 AND `concepts_dir` 存在 AND `entities_dir` 存在 | `[[concepts/概念名]]`、`[[entities/实体名]]` |
| **目录名不同** | 知识库存在但用 `概念/` 而非 `concepts/` | 通过配置 `concepts_dir` 和 `entities_dir` 自定义目录名 |
| **部分联动** | `wiki_root` 非空但无 concepts/entities 目录 | 纯文本关键词（如 `Python, Docker, Redis`） |
| **跨项目路径** | 项目 A 的 dev-extract 写入项目 B 的知识库 | 使用 `wiki_root` 绝对路径，链接仍为 `[[concepts/xxx]]`（Obsidian 通过 vault 解析） |
| **独立运行** | `wiki_root` 为空 | 标签格式（如 `#concept/xxx` `#entity/xxx`） |

---

## 触发条件

- **手动**（可靠）：用户说「记录一下」「提炼」「extract」「存档」等关键词时立即执行
  - 首次触发 → 检查 `.dev-extract.json` → 不存在则发起 3 题问卷 → 生成配置 → 开始提炼
  - 非首次触发 → 直接读取配置 → 开始提炼
- **自动**（依赖平台）：设计上希望在 session 结束时自动扫描，但这需要平台支持 session-end hook 机制。如果平台不支持，请养成手动触发的习惯。

---

## 七类知识

| 类型 | 模板文件 | 最低深度 |
|------|---------|---------|
| 🐛 报错与修复 | `templates/bug-fix.md` | AI 自适应 |
| ⚠️ 踩坑记录 | `templates/pitfall.md` | AI 自适应 |
| 📋 配置配方 | `templates/recipe.md` | AI 自适应 |
| 🔀 方案决策 | `templates/decision.md` | **L3 原理层** |
| 🏗️ 架构设计 | `templates/design.md` | **L3 原理层** |
| 📦 库/框架选型 | `templates/lib-selection.md` | **L3 原理层** |
| ⚡ 性能优化 | `templates/performance.md` | **L3 原理层** |

**L3 原理层必须包含**：底层机制解释 + 横向对比 + 适用边界说明

---

## 通用 frontmatter 字段

所有模板共用字段：

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `type` | string | ✅ | 固定值 `dev-note` |
| `subtype` | string | ✅ | 见上表模板文件名 |
| `tech_stack` | list | ✅ | 如 `[Python, Docker]` |
| `encounter_count` | number | ✅ | 累计遇到次数，初建为 1 |
| `last_encountered` | date | ✅ | 最近一次遇到日期 |
| `status` | string | ✅ | active / outdated / archived |
| `created` | date | ✅ | 首次创建日期 |
| `updated` | date | ✅ | 最后更新日期 |

专用字段：

| 字段 | 适用类型 | 说明 |
|------|---------|------|
| `severity` | bug, pitfall | critical/major/minor 或 warning/caution/info |
| `env` | recipe | 开发环境/生产环境/测试环境 |
| `optimization_type` | perf | SQL优化/缓存/算法/架构/配置 |
| `valid_versions` | 所有（可选） | 已验证版本范围 |
| `expires` | 所有（可选） | 预计过期时间 |

---

## 工作流

### Step 0: 配置检查（首次触发时）

检查 `.dev-extract.json` 是否存在：
- **存在** → 读取配置，进入 Step 1
- **不存在** → 发起 3 题问卷（Q1 存储位置 → Q2 知识库路径 → Q3 链接方式）→ 生成配置文件 → 用户确认 → 进入 Step 1

### Step 1: 扫描对话历史

识别知识点类型（报错、踩坑、配置、决策、设计、选型、性能优化）。

### Step 2: 分类判定

确定知识类型、技术栈标签、提炼深度（L1-L3）。

### Step 2.5: 重复检测（MANDATORY）

新建前在 dev-notes 目录搜索同类知识点：
- 已存在 → 更新原页面，递增 `encounter_count`，追加遇到记录
- 根因不同但表现相似 → 新建页面并互相链接

### Step 2.6: 粒度检查

不同根因 → 独立页面。同一根因的不同表现 → 合并为 1 页。

### Step 3: 深度检查

decision/design/lib/perf → 必须 L3 原理层。其余 AI 自适应。

### Step 4: 加载模板并生成知识页

**按需加载模板**：根据分类结果，读取 `templates/{对应模板}.md` 文件。

根据 `link_style` 渲染链接章节：
- `wiki` → 生成 `[[concepts/xxx]]` 双向链接
- `tag` → 生成 `#concept/xxx` 标签
- `text` → 生成纯文本关键词

### Step 5: 建立链接

根据模式建立链接：
- **完整联动**：链接到 `wiki/concepts/` 概念页、`wiki/entities/` 实体页、`wiki/dev-notes/` 相关知识页
- **部分联动**：仅链接到 `wiki/dev-notes/` 相关知识页，concepts/entities 用纯文本
- **独立运行**：使用标签格式 `#concept/xxx`，不建立 wiki 链接

### Step 6: 更新索引

更新 dev-notes 索引文件，在 wiki 主索引中添加 dev-notes 入口。

### Step 7: 更新日志

在 wiki 日志文件中记录 extract 操作。

---

## 索引格式

生成知识页后，更新索引文件：

```
# 开发知识点索引

## 🐛 报错与修复
| 日期 | 标题 | 技术栈 | 严重程度 | 遇到次数 |
|------|------|--------|---------|---------|

## ⚠️ 踩坑记录
| 日期 | 标题 | 技术栈 | 严重程度 |
|------|------|--------|---------|

## 📋 配置配方
| 日期 | 标题 | 技术栈 | 环境 | 有效期 |
|------|------|--------|------|--------|

## 🔀 方案决策
| 日期 | 标题 | 技术栈 | 状态 |
|------|------|--------|------|

## 🏗️ 架构设计
| 日期 | 标题 | 技术栈 | 状态 |
|------|------|--------|------|

## 📦 库/框架选型
| 日期 | 库名 | 技术栈 | 状态 | 版本 |
|------|------|--------|------|------|

## ⚡ 性能优化
| 日期 | 标题 | 技术栈 | 优化类型 |
|------|------|--------|---------|
```

---

## 过期检查

定期扫描 dev-notes 下所有页面：
- `expires` 已过期 → 标记 `status: outdated`
- `valid_versions` 版本跨度超 2 个大版本 → 建议复核
- 超过 1 年且 `status: outdated` → 建议归档
- 归档：状态改为 `archived`，保留原内容，索引中移入「已归档」分区
