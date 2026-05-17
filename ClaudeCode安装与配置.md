# Claude Code 安装与配置指南

> 本指南面向所有用户（包括非技术用户），帮你从零开始安装和配置 Claude Code。
>
> 无论你是用 CLI 还是桌面端，用官方模型还是国产模型，都能找到对应的方案。

---

## 1. 简介

**Claude Code** 是 Anthropic 推出的终端 AI 编程 Agent。和普通聊天工具不同，它能直接进入你的项目目录、读取文件结构、修改代码、执行命令，像一个「会动手的 AI 程序员」。

> **关于电脑配置**：Claude Code 的核心智能在云端，本地电脑主要负责提供项目文件、执行命令和网络通讯。大多数电脑都能流畅运行，你不需要为它升级硬件。

本文涵盖以下内容：

- **CLI 端安装**（Windows / macOS）
- **桌面端安装**（Claude Code Desktop）
- **CC Switch 配置**（接入国产模型，国内用户必看）
- **常用命令与 CLAUDE.md 配置**
- **VS Code 集成**
- **Skill 加载配置**（与知识库构建器配合使用）

---

## ⏩ 快速通道

> 如果你**已经安装并了解** Claude Code 的基本使用方法，直接跳到 **[第 7 章：Skill 加载配置](#7-skill-加载配置)**。

---

## 2. 安装教程 - CLI 端

### 2.1 前置准备

无论哪种安装方式，都需要先准备好以下环境：

| 项目 | Windows | macOS |
|------|---------|-------|
| **终端** | PowerShell（推荐）或命令提示符 | 终端（Terminal.app） |
| **Node.js** | 安装方法二需要，[nodejs.org](https://nodejs.org/) 下载 LTS 版 | 安装需要，[nodejs.org](https://nodejs.org/) 下载 LTS 版 |
| **Git** | 建议安装 [Git for Windows](https://git-scm.com/download/win) | 系统自带或通过 `xcode-select --install` 安装 |

> **小知识**：Windows 上安装 Git 会附带 Git Bash，Claude Code 在 Windows 上依赖它来执行部分命令。

**装完后先验证**：安装 Node.js 后，打开终端执行：

```bash
node -v
```

能正常返回版本号（如 `v18.20.0`），说明环境已就绪。（如果装了 Git for Windows，也跑一下 `git --version` 确认。）

### 2.2 Windows 安装

#### 方法一：winget 安装（推荐，无需 Node.js）

如果你的 Windows 10/11 自带 `winget`（通常都有），这是最简单的方式：

```powershell
winget install Anthropic.ClaudeCode
```

等待安装完成，然后验证：

```powershell
claude --version
```

看到版本号即安装成功。

> **提示**：如果提示 `claude 不是可识别的命令`，关闭 PowerShell 重新打开再试一次。

#### 方法二：npm 全局安装（跨平台，需 Node.js）

如果你已经安装了 Node.js，也可以用 npm 安装：

```powershell
npm install -g @anthropic-ai/claude-code
```

验证安装：

```powershell
claude --version
```

#### 方法三：官方 PowerShell 脚本（有特殊网络条件时）

如果你的网络可以访问 `claude.ai`，用官方脚本安装最直接：

```powershell
irm https://claude.ai/install.ps1 | iex
```

安装完成后同样用 `claude --version` 验证。

### 2.3 macOS 安装

macOS 推荐使用 npm 全局安装：

```bash
npm install -g @anthropic-ai/claude-code
```

验证安装：

```bash
claude --version
```

> **提示**：如果遇到权限错误，尝试 `sudo npm install -g @anthropic-ai/claude-code`。

### 2.4 首次启动与认证

安装完成后，在终端中输入 `claude` 启动：

```bash
claude
```

首次启动会打开浏览器进入 Anthropic OAuth 登录页面。你需要：

1. **有 Claude 账号**：直接用邮箱登录
2. **没有 Claude 账号**：可以跳转到第 4 章，通过 CC Switch 接入国产模型，无需 Claude 账号

> **小技巧**：第一次启动时，先在电脑上创建一个空的项目文件夹（如 `D:\MyProject`），然后进入该文件夹再启动 Claude Code。不要在根目录（如 `C:\`）启动，会看到太多文件导致「上下文污染」。

---

## 3. 安装教程 - 桌面端（Claude Code Desktop）

> 适合不习惯命令行的用户。桌面版支持图形界面操作，无需记忆命令。

### 3.1 下载安装

访问官网下载页面：**[https://code.claude.com/docs/en/desktop](https://code.claude.com/docs/en/desktop)**

- **Windows**：下载 `.exe` 安装包 → 双击运行 → 按提示完成安装
- **macOS**：下载 `.dmg` 文件 → 打开 → 将应用拖入 `Applications` 文件夹

### 3.2 常规模式（需 Claude 账号）

安装后直接打开应用，使用 Claude 账号登录即可使用。

### 3.3 Developer Mode（免登录，国内用户友好）

最新版 Claude Code Desktop 支持 **Developer Mode**，可以跳过 Claude 账号登录，直接接入第三方模型（包括国产模型和本地 Ollama 部署）。

操作步骤（以 macOS 为例，Windows 类似）：

1. 打开 Claude Code Desktop
2. 菜单栏 **HELP → Troubleshooting → Enable Developer Mode**
3. 点击 **Enable**，应用自动重启
4. 重启后，菜单栏出现 **Developer** 选项
5. 进入 **Developer → Configure Third-Party Inference**
6. 选择 **Gateway** 模式
7. 填写：
   - **Gateway Base URL**：你的模型服务地址（如 Ollama 地址 `http://localhost:11434/v1`）
   - **API Key**：你的 API 密钥
8. 点击 **Apply locally → Relaunch now**
9. 重启后界面出现第三方大模型，即可开始使用

> **适合场景**：国内用户、使用国产模型、本地部署 Ollama、不想注册 Claude 账号的用户。

---

## 4. CC Switch 配置

> 本章为 Claude Code 平台特有内容，国内用户建议完整阅读。

### 4.1 什么是 CC Switch？

**CC Switch**（Claude Code Switch）是一个跨平台图形化桌面工具，专门用于管理 Claude Code、OpenCode 等 AI CLI 工具的模型配置。你可以把它理解成「给 Claude Code 换大脑的遥控器」。

核心功能：

- 一键切换模型（官方 ↔ 第三方）
- 管理多个 API Key 和模型配置
- 内置 50+ 主流模型预设（DeepSeek、MiniMax、智谱 GLM、Kimi 等）
- 还支持 MCP 管理、Prompt 编辑、用量追踪等高级功能

### 4.2 安装 CC Switch

**官方网址**：[ccswitch.io](https://ccswitch.io)

从 GitHub Releases 页面下载最新版本：

```text
https://github.com/farion1231/cc-switch/releases
```

- **Windows**：下载 `.exe` 安装包，双击运行，一路 Next 完成安装
- **macOS**：下载 `.dmg` 文件，安装后可直接使用（已通过 Apple 公证）

安装完成后，可以在开始菜单（Windows）或启动台（macOS）找到 **CC Switch** 并打开。

### 4.3 获取 API Key（以 DeepSeek 为例）

> 其他国产模型（MiniMax、智谱、Kimi 等）操作类似，去对应开放平台获取即可。

1. 打开 DeepSeek 开放平台：**[https://platform.deepseek.com](https://platform.deepseek.com)**
2. 注册账号并登录
3. 进入 **API Keys** 页面，点击「创建 API Key」
4. 复制生成的 Key（形如 `sk-xxxxxxxxxxxxxxxx`）

> **提示**：首次使用通常有免费额度，可以直接体验。
>
> **另外**：部分国产模型厂商也提供了自己的快捷接入方式，例如智谱可以直接运行以下命令（无需 CC Switch）：
> ```
> npx @z_ai/coding-helper
> ```
> 它会引导你完成配置。具体请参考对应厂商的官方文档。

### 4.4 在 CC Switch 中配置模型

1. 打开 **CC Switch** 应用
2. 在「Claude」选项下，点击右上角的 **加号（+）** → 新增模型配置
3. 从预设列表中选择你要用的模型（如 DeepSeek Chat / DeepSeek Reasoner）
4. 填写上一步获取的 **API Key**
5. 其他参数保持默认即可，点击「添加」
6. 在主界面点击刚添加的模型，使其变为「启用」状态

![image-20260517004423198](ClaudeCode安装与配置.assets/image-20260517004423198.png)

![image-20260517005041931](ClaudeCode安装与配置.assets/image-20260517005041931.png)

![image-20260517005103185](ClaudeCode安装与配置.assets/image-20260517005103185.png)

> **💡 使用小贴士**：![image-20260517005103185](ClaudeCode安装与配置.assets/image-20260517005103185.png
>
> - **热切换**：Claude Code 支持供应商热切换——在 CC Switch 中切换模型后，无需重启终端，直接在对话中输入 `/model` 即可选择新模型
> - **切换回官方登录**：如果需要切回 Claude 官方模型，在 CC Switch 的预设供应商中选择「官方」→ 切换过去 → 在 Claude Code 中执行一遍 Log out / Log in 即可
> - **统一 MCP 管理**：CC Switch 还支持统一管理 Claude Code 的 MCP 服务器配置，一个面板管理多个应用

### 4.5 启动 Claude Code

#### 创建项目文件夹

在电脑上任意位置新建一个文件夹：

- **Windows**：`D:\MyProject` 或 `C:\Users\你的用户名\Code\project1`
- **macOS**：`~/Documents/MyProject`

#### 启动 Claude Code

打开终端进入该文件夹，然后执行：

```bash
claude
```

出现小螃蟹图标 🦀，说明启动成功！

![image-20260517005420947](ClaudeCode安装与配置.assets/image-20260517005420947.png)

> **开发模式**：如果不想每次都点权限确认，可以用开发模式启动（注意风险）：
>
> ```bash
> claude --dangerously-skip-permissions
> ```
>
> 删除文件、改密钥等高风险操作依然会询问你。

---

## 5. 常用命令与进阶用法

### 5.1 常用命令速查

| 命令 | 说明 |
|------|------|
| `claude` | 启动 Claude Code |
| `exit` 或 `Ctrl+C` | 退出 Claude Code |
| `/help` | 查看所有可用命令 |
| `/model` | 切换当前使用的模型 |
| `claude --dangerously-skip-permissions` | 开发模式启动（跳过权限确认） |

### 5.2 CLAUDE.md 配置

`CLAUDE.md` 是 Claude Code 的配置文件，用来设定你和 AI 之间的协作规则。它分为两种：

| 类型 | 路径 | 作用范围 |
|------|------|---------|
| **全局** | `%USERPROFILE%\.claude\CLAUDE.md`（Win）<br>`~/.claude/CLAUDE.md`（Mac） | 所有项目生效 |

> **💡 简单理解**：`%USERPROFILE%` 是 Windows 的系统变量，指向你的用户文件夹，通常就是 `C:\Users\你的用户名`。后面跟着的 `\.claude\CLAUDE.md` 就是在该文件夹下找到 `.claude` 子文件夹，再找到里面的 `CLAUDE.md` 文件。
>
> macOS 中的 `~` 也是同样的意思，指向你的用户文件夹 `/Users/你的用户名`。

#### 配置全局 CLAUDE.md（推荐新手第一天就配置好）

**Windows**：

下面的命令会自动在你的用户文件夹下创建 `.claude\CLAUDE.md`（路径大致为 `C:\Users\你的用户名\.claude\CLAUDE.md`）：

```powershell
# 创建 .claude 文件夹
mkdir $env:USERPROFILE\.claude -Force

# 用记事本编辑 CLAUDE.md
notepad $env:USERPROFILE\.claude\CLAUDE.md
```

**macOS**：

```bash
# 创建 .claude 文件夹
mkdir -p ~/.claude

# 用文本编辑打开
open -e ~/.claude/CLAUDE.md
```

粘贴以下模板（可根据自己喜好修改）：

```markdown
## 关于我
[你的称呼，例如：新手开发者 / 产品经理 / 学生]
我用 Claude Code 做 [主要用途，比如：写 Python 脚本 / 学习代码 / 开发小工具]

## 沟通风格
- 使用中文交流，代码和命令用英文
- 直接给我答案，不要夸我「这是个好问题」
- 如果我的想法有问题，直接指出来

## 红线（必须问我）
以下操作必须先问我才能执行：
- 删除任何文件或文件夹
- 修改 .env、密钥、token
- 修改数据库结构
- git push、强制重置代码
- 安装全局依赖

## 工程习惯
- 改动代码后，自动运行测试或验证命令
- 不要把密钥、密码写进代码里
- 大改动之前，先给我方案确认
```

保存后，以后每次启动 Claude Code，这些规则都会自动生效。

#### 配置项目级 CLAUDE.md（可选）

进入你的项目文件夹，启动 Claude Code，然后输入：

> 「请根据这个项目的结构，帮我生成一份 CLAUDE.md」

它会自动扫描并创建，你只需要确认或稍作修改即可。

### 5.3 第一次试手建议

> 如果你是第一次用 Claude Code，建议先从小任务开始建立感觉，不要一来就把大型遗留项目丢给它。

**推荐的小任务**：
- 生成一个简单的 HTML 页面或小游戏 Demo
- 写一个小功能（如「给我写一个计算器」）
- 解释一段你看不懂的代码
- 修一个能稳定复现的小 Bug

**三个原则**：
1. **任务越具体，结果越稳定**——不要说「帮我优化一下」，要说清楚改哪个模块、目标是什么、有什么边界
2. **先做小任务，再做大的**——先建立节奏感，再逐步放大范围
3. **让它做事，也要学会验证结果**——你负责判断方向和验收，它负责推进执行

### 5.4 常见问题

**Q：电脑配置一般，真的能跑 Claude Code 吗？**

A：可以。Claude Code 的核心智能在云端，本地只负责运行环境、文件访问和命令执行，对硬件要求不高。真正影响体验的是网络和环境配置，不是显卡或内存。

**Q：Claude Code 和 Cursor，到底先用哪个？**

A：两者分工不同。Cursor 更适合在编辑器里边写边改，Claude Code 更适合把完整任务交给 AI 自主执行。新手可以先理解两者的区别，再根据自己的使用场景选择。

---

## 6. VS Code 集成

> 如果你不习惯纯终端操作，可以把 Claude Code 接进 VS Code 使用。

### 6.1 安装 VS Code

如果还没有 VS Code，从[VS Code官网](https://code.visualstudio.com/)下载安装

![image-20260517013615393](ClaudeCode安装与配置.assets/image-20260517013615393.png)

### 6.2安装 Claude Code 扩展

VS Code 也有官方的 Claude Code 扩展，安装后可以在侧边栏直接与 Claude 交互：

1. 打开 VS Code 扩展面板（`Ctrl + Shift + X`）
2. 搜索「Claude Code」
3. 安装 Anthropic 官方扩展

![image-20260517015708248](ClaudeCode安装与配置.assets/image-20260517015708248.png)

4. 安装后界面右上角会出现入口。
5. 打开后的界面就像下面这样，如果在 CC Switch 中配置过模型，就可以直接使用了

![image-20260517015845923](ClaudeCode安装与配置.assets/image-20260517015845923.png)

---

## 7. Skill 加载配置

> 如果你在使用「知识库构建器」项目，需要通过 Skill 加载知识库工作流。

本项目包含两个 Skill 文件：

| 文件 | 用途 |
|------|------|
| `skills/knowledge-base-builder/SKILL.md` | 初始化知识库、问卷引导、资料分类、Wiki 维护 |
| `skills/dev-extract/SKILL.md` | 从开发对话中提炼知识点 |

要让 Claude Code 使用这些文件，有两种方式：

---

### 方式 A：在 CLAUDE.md 中写下文件路径（最简单）

**原理**：Claude Code 每次启动时会自动读取项目中的 `CLAUDE.md` 文件，把它当作工作指南。如果你在 `CLAUDE.md` 里写「这里有 Skill 文件」，Claude 就会知道它们的存在。当你说到相关关键词时，它会主动去读取和执行。

#### 操作步骤

1. 打开项目文件夹
2. 找到（或新建）`CLAUDE.md` 文件
3. 在文件中写入以下内容：

```markdown
# 知识库构建器

## Skill 文件

本项目包含以下 Skill 文件。当用户提到相关需求时，请读取对应的文件并按照流程执行：

- `skills/knowledge-base-builder/SKILL.md`
  - 用途：初始化知识库、问卷引导、资料分类、Wiki 维护
  - 用户可能说：「初始化知识库」「构建知识库」「帮我建一个知识库」

- `skills/dev-extract/SKILL.md`
  - 用途：从开发对话中提炼知识点
  - 用户可能说：「记录一下」「提炼」「extract」「把这个记到知识库里」
```

4. 保存文件

**之后怎么用？**

当你对 Claude 说「帮我初始化知识库」，Claude 会主动去读取 `skills/knowledge-base-builder/SKILL.md` 并按流程执行。你也可以直接说：

```
请读取 skills/knowledge-base-builder/SKILL.md 并按照流程执行
```

> **优点**：无需额外操作，SKILL.md 文件留在项目源码中，随 Git 一起管理。
> **局限**：没有 `/` 命令快捷键，全靠 Claude 理解 CLAUDE.md 中的指令。

---

### 方式 B：安装为原生 Skill（可使用 / 命令调用）

**原理**：Claude Code 会自动扫描特定目录下的 SKILL.md 文件，将其注册为可调用的 `/` 命令。

原生 Skill 分为两个层级：

| 层级 | 存放位置 | 作用范围 |
|------|---------|---------|
| **项目级**（local） | 项目根目录的 `.claude/skills/` | 仅当前项目可用 |
| **用户级**（user，即全局） | 用户目录的 `.claude/skills/` | 所有项目可用 |

两个层级的结构完全一样，只是存放位置不同：

```
项目级 → 项目文件夹/.claude/skills/<技能名>/SKILL.md
用户级 → C:\Users\你的用户名\.claude\skills\<技能名>/SKILL.md  (Windows)
         ~/.claude/skills/<技能名>/SKILL.md                  (macOS/Linux)
```

#### 安装到项目级（仅当前项目可用）

**命令行方式**：

```bash
cd 知识库构建器

# 创建目录（每个 Skill 一个独立子目录）
mkdir -p .claude/skills/knowledge-base-builder
mkdir -p .claude/skills/dev-extract

# 复制 SKILL.md 到对应的子目录
cp skills/knowledge-base-builder/SKILL.md .claude/skills/knowledge-base-builder/
cp skills/dev-extract/SKILL.md .claude/skills/dev-extract/
```

**图形界面方式（Windows）**：

1. 打开项目文件夹，按 **Ctrl + H** 显示隐藏文件
2. 找到（或新建）`.claude` 文件夹
3. 在 `.claude` 下新建 `skills` 文件夹
4. 在 `skills` 下分别新建 `knowledge-base-builder` 和 `dev-extract` 两个子文件夹
5. 回到项目的 `skills/` 文件夹，将两个 SKILL.md 分别复制到对应的子文件夹中

**图形界面方式（macOS）**：

1. 打开项目文件夹，按 **Cmd + Shift + .** 显示隐藏文件
2. 找到或创建 `.claude/skills/` 目录
3. 在 `skills` 下分别新建对应名称的子文件夹
4. 复制 SKILL.md 文件到对应的子文件夹

#### 安装到用户级（所有项目可用）

如果你希望知识库 Skill 在所有 Claude Code 项目中都能使用，可以将其安装到用户级目录。

**命令行方式（Windows PowerShell）**：

```powershell
# 创建目录
mkdir $env:USERPROFILE\.claude\skills\knowledge-base-builder -Force
mkdir $env:USERPROFILE\.claude\skills\dev-extract -Force

# 复制 SKILL.md
copy 知识库构建器\skills\knowledge-base-builder\SKILL.md $env:USERPROFILE\.claude\skills\knowledge-base-builder\
copy 知识库构建器\skills\dev-extract\SKILL.md $env:USERPROFILE\.claude\skills\dev-extract\
```

**命令行方式（macOS/Linux）**：

```bash
# 创建目录
mkdir -p ~/.claude/skills/knowledge-base-builder
mkdir -p ~/.claude/skills/dev-extract

# 复制 SKILL.md
cp 知识库构建器/skills/knowledge-base-builder/SKILL.md ~/.claude/skills/knowledge-base-builder/
cp 知识库构建器/skills/dev-extract/SKILL.md ~/.claude/skills/dev-extract/
```

**图形界面方式（Windows）**：

1. 打开文件资源管理器，在地址栏输入 `%USERPROFILE%\.claude\skills` 回车
2. 如果没有 `skills` 文件夹，手动新建一个
3. 在 `skills` 下分别新建 `knowledge-base-builder` 和 `dev-extract` 两个子文件夹
4. 将对应 SKILL.md 文件复制进去

**图形界面方式（macOS）**：

1. 打开 Finder，按 **Cmd + Shift + G**，输入 `~/.claude/skills` 回车
2. 如果没有 `skills` 文件夹，手动新建一个
3. 在 `skills` 下分别新建对应名称的子文件夹
4. 复制 SKILL.md 文件到对应的子文件夹

#### 安装后如何使用

无论安装在项目级还是用户级，安装后 Claude Code 都会自动发现这些 Skill。在对话中直接输入斜杠命令即可：

- `/knowledge-base-builder` — 运行知识库初始化流程
- `/dev-extract` — 运行知识提炼流程

Claude Code 也会在相关场景下自动加载它们。

> **项目级 vs 用户级 怎么选？**
>
> - 如果这个 Skill 只在你一个项目上使用 → **项目级**就够了
> - 如果你希望知识库 Skill 在**所有 Claude Code 项目**中都能用（比如你同时维护多个知识库项目）→ 装到**用户级**
>

---

## 8. 推荐插件

以下是 Claude Code 生态中推荐安装的插件：

| 插件 | 说明 | 安装方式 |
|------|------|---------|
| **oh-my-claudecode** | 多 Agent 编排工具，提供 `/autopilot`、`/team`、`/ralph` 等高级工作流 | 插件市场安装：`/plugin marketplace add https://github.com/Yeachan-Heo/oh-my-claudecode` |
| **superpowers** | 完整开发方法论，覆盖从需求分析到代码交付的全流程 | 官方市场安装：`/plugin install superpowers@claude-plugins-official` |
| **claude-mem** | 跨会话持久记忆，自动捕获并注入相关上下文 | 插件市场安装：`/plugin marketplace add thedotmack/claude-mem` |

> 完整插件列表详见 [插件与工具推荐.md](./插件与工具推荐.md)。

---

## 9. 国内用户提示

> 💡 **国内用户遇到问题？**
>
> 如果你在国内使用 Claude Code，CC Switch 配置见上文第 4 章，其他通用问题（npm 镜像源、代理配置、国内模型推荐、Ollama 本地部署等）详见：
>
> 👉 [常见问题与解决方案.md](./常见问题与解决方案.md)

---

## 10. 注意事项

- **Skill 加载方式的选择**：方式 A（CLAUDE.md 中写路径）不需要额外配置，SKILL.md 随项目源码管理；方式 B（原生 Skill）有 `/` 命令快捷键，但 SKILL.md 更新后需要手动重新复制到 `.claude/skills/` 目录
- **方式 A 依赖于 CLAUDE.md**：如果你选择方式 A，确保 `CLAUDE.md` 文件位于项目根目录，并且写入了正确的 Skill 文件路径
- **上下文污染**：不要在根目录（如 `C:\`）启动 Claude Code，为每个项目建独立文件夹
- **隐藏文件夹提示**：以 `.` 开头的文件夹默认隐藏
  - Windows：在文件资源管理器中点击「查看」→ 勾选「隐藏的项目」
  - macOS：按 **Cmd + Shift + .**
- **权限确认**：首次使用时，Claude Code 会请求文件读写和命令执行权限，按提示确认即可
- **文件读写权限**：使用知识库构建器时，确保允许 Claude Code 读写 `原始资料/` 和 `wiki/` 目录
