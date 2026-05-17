> **渐进式加载**：本文件为按需加载层。仅在需要执行 Extract（提炼开发知识点）时，由核心 AGENTS.md 或 CLAUDE.md 指示读取。

---

## Extract 工作流

> 从开发对话中提炼结构化的开发知识点，存入 wiki/dev-notes/ 分区

### 触发条件

**手动触发**: 用户在对话中说「记录一下」「提炼」「extract」「存档」等关键词
**自动触发（依赖平台）**: 设计上希望在 session 结束时自动扫描，但这需要平台支持 session-end hook 机制。目前阶段建议以手动触发为主。

### 七类知识

| 类型 | 子目录 | 提炼深度 |
|------|--------|---------|
| 🐛 报错与修复 | `bugs/` | AI自适应 |
| ⚠️ 踩坑记录 | `pitfalls/` | AI自适应 |
| 📋 配置配方 | `recipes/` | AI自适应 |
| 🔀 方案决策 | `decisions/` | **L3 原理层** |
| 🏗️ 架构设计 | `designs/` | **L3 原理层** |
| 📦 库/框架选型 | `libs/` | **L3 原理层** |
| ⚡ 性能优化 | `perf/` | **L3 原理层** |

> L3 原理层必须包含：底层机制解释 + 横向对比 + 适用边界说明

### 流程步骤

```
Step 1: 扫描对话历史
  → 识别以下类型的知识点:
    - 报错信息 + 修复过程
    - 踩坑/陷阱
    - 环境配置步骤
    - 技术方案对比讨论
    - 架构设计讨论
    - 库/框架选型讨论
    - 性能优化过程

Step 2: 分类判定
  → 确定知识类型（bug/pitfall/recipe/decision/design/lib/perf）
  → 确定技术栈标签
  → 确定提炼深度（L1-L3）

Step 2.5: 重复检测（MANDATORY - 新建前必执行）
  → 在 wiki/dev-notes/ 中搜索是否已有同类知识点
  → 搜索维度: 相同错误信息（bug）、相同陷阱描述（pitfall）、相同配置目标（recipe）
  → 如果已存在:
    - 更新原页面，不要新建
    - 递增 encounter_count
    - 更新 last_encountered
    - 在「遇到记录」表格中追加新行
    - 如果有新信息（新版本表现、新修复方式），更新对应章节
  → 如果不确定是否算重复:
    根因相同但表现不同 → 更新原页面（追加复现条件）
    根因不同但表现相似 → 新建页面，互相链接

Step 2.6: 粒度检查
  → 一次对话中可能涉及多个独立知识点
  → 判断标准: 不同根因 → 独立页面；不同技术栈/模块 → 独立页面
  → 示例: 一次对话修了 3 个 bug，3 个不同根因 → 3 个页面
  → 示例: 3 个 bug 都因同一配置错误 → 1 个页面

Step 3: 深度检查
  → 如果是 decision/design/lib/perf → 必须达到 L3 原理层
  → 如果是 bug/pitfall/recipe → AI 根据复杂度自适应判断深度
  → L3 必须包含: 底层机制解释 + 横向对比 + 适用边界

Step 4: 生成知识页
  → 路径: wiki/dev-notes/{子目录}/{YYYY-MM-DD-标题}.md
  → 使用对应模板（见 skills/dev-extract/SKILL.md）
  → frontmatter 必填字段完整

Step 5: 建立链接
  → 链接到 wiki/concepts/ 中的相关概念页
  → 链接到 wiki/dev-notes/ 中的相关知识页
  → 链接到 wiki/entities/ 中的相关实体页

Step 6: 更新索引
  → 更新 wiki/dev-notes/index.md
  → 在 wiki/index.md 中添加 dev-notes 入口

Step 7: 更新日志
  → 在 wiki/log.md 记录 extract 操作
```

### Extract Prompt 模板

```markdown
## Extract任务

### 输入
- 对话历史: 当前 session 的完整对话
- 目标: 提炼其中的开发知识点

### 执行
1. 扫描对话历史，识别知识点类型（bug/pitfall/recipe/decision/design/lib/perf）
2. 对每个知识点:
   - 在 wiki/dev-notes/ 中搜索是否已有同类页面（重复检测）
   - 确定提炼深度（L3 必须: decision/design/lib/perf）
   - 生成知识页: wiki/dev-notes/{子目录}/{YYYY-MM-DD-标题}.md
   - 建立与 concepts/entities/dev-notes 的链接
3. 更新 wiki/dev-notes/index.md
4. 更新 wiki/index.md
5. 更新 wiki/log.md

### 要求
- 严格使用 dev-extract skill 中的模板
- frontmatter 必须完整（含 encounter_count、last_encountered、status）
- 同一根因的多个表现 → 更新已有页面
- 不同根因 → 独立页面
- L3 原理层必须包含: 底层机制 + 横向对比 + 适用边界
```
