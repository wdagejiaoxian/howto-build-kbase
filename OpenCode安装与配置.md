# OpenCode 平台配置指南

---

## 1. OpenCode 简介

OpenCode 是一款开源的 AI 编程代理工具，由 Anomaly 团队开发，采用 MIT 开源协议。它支持接入 75+ 种模型提供商（Claude、GPT、Gemini、智谱 AI、DeepSeek 等），可以自由切换模型，甚至可以使用本地模型（Ollama）。

**三种使用方式**：

| 方式 | 说明 | 适合人群 |
|------|------|---------|
| **终端界面（TUI）** | 在命令行中运行，轻量高效 | 开发者、熟悉命令行的用户 |
| **桌面应用** | 图形化界面，直观易用 | 新手、非技术用户（推荐） |
| **IDE 扩展** | VS Code / Cursor / Zed 等编辑器插件 | 习惯在编辑器中工作的用户 |

---

## ⏩ 快速通道

> 如果你**已经安装并了解** OpenCode 的基本使用方法，直接跳到 **[第 7 章：Skill 加载方式](#7-skill-加载方式)**。

---

## 2. 安装教程 - CLI 端

### 前置条件：安装 Node.js（仅方式 B/C 需要）

> 💡 **提示**：如果你使用方式 A（官方脚本）或方式 E（手动下载二进制），**不需要安装 Node.js**，因为 OpenCode 已打包为独立可执行文件。只有使用 npm 或 Bun 安装时才需要。

OpenCode 的 npm/bun 安装方式基于 Node.js 运行，需要先安装 Node.js（推荐 LTS 版本）。

#### 方式 A：通过安装包安装

1. 前往 [Node.js 官网](https://nodejs.org/zh-cn/download)
2. 下载带有 **LTS** 后缀的 Windows 安装程序（.msi 文件）
3. 双击下载的文件，一路点击 **Next** 即可完成安装
4. 验证安装：打开 cmd 或 PowerShell，输入 `node -v`，显示版本号即成功

#### 方式 B：通过 NVM 安装

使用 NVM（Node Version Manager）可以方便地管理多个 Node.js 版本。

### 安装 OpenCode CLI

OpenCode 提供多种安装方式，选择最适合你的一种：

#### 方式 A：官方安装脚本（推荐，最快）

适用于 macOS、Linux、WSL2（Windows 子系统）。打开终端，运行：

```bash
curl -fsSL https://opencode.ai/install | bash
```

该脚本会自动检测你的操作系统和架构，下载正确的二进制文件并添加到 PATH。

> 💡 **Windows 用户注意**：如果你的电脑没有 WSL2，请使用方式 B（npm 安装）或方式 D（直接下载二进制）。

#### 方式 B：通过 npm 安装（Windows 原生推荐）

打开 **cmd** 或 **PowerShell**，输入：

```bash
npm i -g opencode-ai
```

> 💡 **国内用户提示**：如果 npm 下载速度慢，可以先切换 npm 镜像源。详见 [常见问题与解决方案.md](./常见问题与解决方案.md)。

#### 方式 C：通过 Bun 安装

如果你已安装 Bun：

```bash
bun add -g opencode-ai
```

#### 方式 D：通过 Homebrew 安装（macOS）

```bash
brew install anomalyco/tap/opencode
```

#### 方式 E：手动下载二进制文件

前往 [GitHub Releases](https://github.com/anomalyco/opencode/releases/latest) 下载对应平台的二进制文件：

| 平台 | 文件名 |
|------|--------|
| macOS (Apple Silicon) | `opencode-darwin-arm64.zip` |
| macOS (Intel) | `opencode-darwin-x64.zip` |
| Windows (x64) | `opencode-windows-x64.zip` |
| Linux (x64) | `opencode-linux-amd64.tar.gz` |

下载后解压，将 `opencode` 可执行文件移动到 PATH 目录（如 `/usr/local/bin` 或 `C:\Windows\System32`）。

### 验证安装

```bash
opencode --version
```

显示版本号（如 `1.1.64`）即表示安装成功。

### 启动 OpenCode

```bash
# 在项目目录中运行
opencode
```

---

## 3. 安装教程 - 桌面端

### 方式 A：从官网下载（推荐新手）

1. 打开浏览器，访问 [opencode.ai/download](https://opencode.ai/download)
2. 找到 **OpenCode Desktop (Beta)** 区域
3. 根据你的操作系统点击下载：
   - **Windows 用户**：点击 **Windows (x64)** 按钮，下载 `.exe` 安装包
   - **macOS 用户**：点击 **macOS (Apple Silicon)** 或 **macOS (Intel)**
   - **Linux 用户**：点击 **Linux (.deb)** 或 **Linux (.rpm)**
4. 下载完成后，双击安装包进行安装
   - **Windows**：双击 `.exe` 文件，按提示完成安装
   - **macOS**：双击 `.dmg` 文件，将 OpenCode 拖入 Applications 文件夹
   - **Linux**：使用包管理器安装（`sudo dpkg -i xxx.deb` 或 `sudo rpm -i xxx.rpm`）

### 方式 B：通过包管理器安装

#### macOS（Homebrew）

```bash
brew install --cask opencode-desktop
```

#### Windows（Scoop）

```bash
scoop bucket add extras
scoop install extras/opencode-desktop
```

### 首次使用桌面版

1. 打开 OpenCode Desktop
2. 点击 **File → Open Folder**，选择你的项目文件夹
3. 首次打开后，右侧会显示 AI 对话面板，可以开始与 AI 对话

### 桌面版的核心优势

- **图形化界面** ：无需记忆命令，直接通过界面操作
- **实时预览** ：AI 修改代码时可以实时预览变化
- **文件管理** ：内置文件浏览器，方便项目管理
- **多会话支持** ：可以同时管理多个项目会话

---

## 4. 模型配置

OpenCode 支持 75+ 模型提供商。你可以直接使用内置的免费模型（OpenCode Zen），也可以接入第三方模型提供商。

### OpenCode Zen 内置免费模型（无需 API 密钥）

OpenCode 自带 **OpenCode Zen** 服务，安装后即可直接使用多个免费模型，**无需配置任何 API 密钥**。

| 模型 | 特点 | 隐私说明 | 适合场景 |
|------|------|---------|---------|
| **GPT-5 Nano** | OpenAI 最小最快模型，**永久免费** | 数据不用于训练 | 日常轻量任务、隐私敏感代码 |
| **Big Pickle** | 隐形模型，专为编程代理优化 | 限时免费期间数据可能用于改进 | 复杂编程、代码审查 |
| **Nemotron 3 Super Free** | NVIDIA 开源模型，**100 万 token 上下文** | 限时免费期间数据可能用于改进 | 代码生成、日常编程 |
| **MiMo V2 Flash Free** | 小米开源模型，速度极快 | 限时免费期间数据可能用于改进 | 编程和推理任务 |
| **MiniMax M2.5 Free** | MiniMax 开源模型，擅长编程和工具调用 | 限时免费期间数据可能用于改进 | 学习和探索 |
| **Qwen3.6 Plus Free** | 阿里通义千问，处理复杂任务能力强 | 限时免费期间数据可能用于改进 | 复杂任务、非敏感项目 |

> 💡 **免费额度**：OpenCode Zen 免费层每天 **100 次请求**，支持多会话。无需信用卡即可使用。

> ⚠️ **注意**：部分限时免费模型在免费期间收集的数据可能用于模型改进。如果处理敏感/私有代码，建议使用 **GPT-5 Nano**（永久免费且数据不用于训练）或付费模型。

### 如何配置 API 密钥

> 💡 **提示**：如果你使用 OpenCode Zen 内置免费模型，**无需配置 API 密钥**，安装后即可直接使用。以下配置步骤仅适用于第三方模型提供商。

#### 桌面端操作

1. 打开 OpenCode Desktop
2. 点击设置
3. 选择提供商（如 智谱 AI、硅基流动等）
4. 输入你的 API 密钥
6. 点击 **保存**

#### CLI/TUI 端操作

**在 TUI（终端界面）内**：

1. 启动 OpenCode 后，输入 `/connect` 命令
2. 从列表中选择目标提供商（如 Anthropic、OpenAI、智谱 AI 等）
3. 按提示输入 API 密钥
4. 输入 `/model` 查看当前可用模型列表，确认新提供商已生效

**在终端命令行中（不进入 TUI）**：

```bash
# 添加提供商 API 密钥
opencode auth login

# 查看已认证的提供商列表
opencode auth list

# 移除某个提供商的认证
opencode auth logout
```

### 如何切换模型

#### 桌面端

点击右侧面板顶部的 **模型名称** → 从下拉列表中选择目标模型。

#### CLI/TUI 端

输入 `/model` 命令，从列表中选择目标模型。

---

## 5. 常用指令与进阶用法

### 常用命令速查

#### Slash 命令（在 OpenCode 提示符中输入）

| 命令 | 说明 |
|------|------|
| `/help` | 显示帮助对话框和可用命令列表 |
| `/models` | 打开模型选择器，查看/切换模型 |
| `/connect` | 添加模型提供商并配置 API 密钥 |
| `/new`（别名 `/clear`） | 开始新会话，当前会话保留在历史记录中 |
| `/sessions`（别名 `/resume`、`/continue`） | 列出并切换会话 |
| `/compact`（别名 `/summarize`） | 压缩/总结当前会话，释放上下文空间 |
| `/mcp` | 列出已配置的 MCP 服务器及其连接状态 |
| `/themes` | 列出并切换主题 |
| `/undo` | 撤销最后一条用户消息及 AI 的文件修改 |
| `/redo` | 恢复被 `/undo` 撤销的消息和文件修改 |
| `/share` | 将当前会话分享为公开链接 |
| `/export` | 将当前对话导出为 Markdown |
| `/init` | 引导式创建或更新项目级 `AGENTS.md` |
| `/exit`（别名 `/quit`、`/q`） | 退出 OpenCode |

#### 快捷键（Ctrl+X 为默认引导键）

| 快捷键 | 说明 |
|--------|------|
| `Ctrl+X N` | 新会话 |
| `Ctrl+X L` | 列出会话 |
| `Ctrl+X C` | 压缩会话 |
| `Ctrl+X M` | 模型选择器 |
| `Ctrl+X T` | 主题选择器 |
| `Ctrl+X A` | Agent 选择器 |
| `Ctrl+X U` | 撤销 |
| `Ctrl+X R` | 重做 |
| `Ctrl+X Q` | 退出 |
| `Ctrl+X E` | 打开外部编辑器编写长消息 |
| `Ctrl+X X` | 导出对话 |

### 进阶技巧

- **多会话管理**：`/sessions` 列出所有会话并切换，`/new` 创建新会话（旧会话保留在历史中）
- **自动压缩**：当对话接近模型上下文窗口限制（95%）时，OpenCode 可自动总结会话
- **撤销/重做**：`/undo` 撤销最后一条消息及 AI 的文件修改，`/redo` 恢复，可连续使用
- **分享对话**：`/share` 生成公开链接（opncd.ai/s/...），方便与他人分享
- **导出对话**：`/export` 将当前对话导出为 Markdown 文件
- **引导式初始化**：`/init` 帮助创建或更新项目级 `AGENTS.md` 文件
- **自定义命令**：在 `~/.config/opencode/commands/` 或 `.opencode/commands/` 中添加 Markdown 文件即可创建自定义命令
- **快捷键**：TUI 使用 `Ctrl+X` 作为默认引导键，按 `Ctrl+?` 或 `?` 查看完整快捷键列表

---

## 6. AGENTS.md 配置

> `AGENTS.md` 是 OpenCode 的**项目规则文件**，相当于 Claude Code 的 `CLAUDE.md`。它告诉 AI 你的项目结构、工程习惯、红线规则等。

### 6.1 什么是 AGENTS.md？

启动 OpenCode 时，AI 会自动读取项目根目录的 `AGENTS.md`，把它当作工作指南。你可以在里面写：

- 项目结构和架构说明（让 AI 快速理解项目）
- 构建、测试、lint 命令（AI 会自动按指定方式验证）
- 沟通风格和红线规则（哪些操作必须问你）
- 项目特有的约定和注意事项

### 6.2 初始化 AGENTS.md

OpenCode 提供了 `/init` 命令，可以自动扫描项目文件生成 `AGENTS.md`：

```bash
# 在项目目录中启动 OpenCode，然后输入：
/init
```

AI 会扫描项目中的重要文件，询问几个关键问题（如果信息不足），然后自动创建或更新 `AGENTS.md`。

> **推荐提交到 Git**：`AGENTS.md` 建议提交到版本控制，方便团队成员共享工作规范。

### 6.3 手动编写示例

你也可以手动创建 `AGENTS.md`。以下是一个通用模板：

```markdown
# 项目规则

## 沟通风格
- 使用中文交流，代码和命令用英文
- 直接给出答案，不要夸我「这是个好问题」
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

### 6.4 全局 AGENTS.md

如果你希望规则对所有项目生效，可以在全局配置目录下创建 `AGENTS.md`：

- **Windows**：`%USERPROFILE%\.config\opencode\AGENTS.md`  
  > 💡 简单理解：`%USERPROFILE%` 是你的用户文件夹，通常就是 `C:\Users\你的用户名`。所以完整路径大致是 `C:\Users\你的用户名\.config\opencode\AGENTS.md`

- **macOS/Linux**：`~/.config/opencode/AGENTS.md`  
  > 💡 `~` 也是你的用户文件夹，即 `/Users/你的用户名`

### 6.5 优先级规则

多个配置文件并存时，优先级如下（高 → 低）：

```
项目级 AGENTS.md  >  全局 AGENTS.md  >  项目级 CLAUDE.md（兼容）
```

> 如果你同时有 `AGENTS.md` 和 `CLAUDE.md`，OpenCode 只会读取 `AGENTS.md`。如果想保留 Claude Code 兼容性，可以在 `AGENTS.md` 中用 `include` 方式引用 `CLAUDE.md`，或通过 `opencode.json` 配置自定义指令文件。

---

## 7. Skill 加载方式

OpenCode 支持两种 Skill 加载方式：

### 方式 A：项目级加载（推荐）

将 Skill 目录复制到项目的 `.opencode/skills/` 下。

#### 图形界面操作（Windows）

1. 打开「知识库构建器」文件夹
2. 进入 `skills` 文件夹，选中 `knowledge-base-builder` 和 `dev-extract` 两个文件夹
3. 右键 → **复制**
4. 打开你的项目文件夹（即你要构建知识库的文件夹）
5. 按 `Ctrl + H` 显示隐藏文件（`.opencode` 是隐藏文件夹）
6. 如果已有 `.opencode` 文件夹 → 进入 `.opencode` → 进入 `skills` → 右键 → **粘贴**
7. 如果没有 `.opencode` 文件夹 → 在项目文件夹中右键 → **新建** → **文件夹** → 命名为 `.opencode` → 进入 → 再新建 `skills` 文件夹 → 右键 → **粘贴**

#### 图形界面操作（macOS）

1. 打开「知识库构建器」文件夹
2. 进入 `skills` 文件夹，选中两个 Skill 文件夹
3. 按 `Cmd + C` 复制
4. 打开你的项目文件夹
5. 按 `Cmd + Shift + .` 显示隐藏文件
6. 找到 `.opencode/skills/` 文件夹 → 按 `Cmd + V` 粘贴
7. 如果没有 `.opencode` 文件夹 → 新建文件夹命名为 `.opencode` → 进入 → 新建 `skills` → 粘贴

#### 命令行操作（可选，技术用户）

```bash
# 在项目根目录下执行
mkdir -p .opencode/skills
cp -r 知识库构建器/skills/knowledge-base-builder .opencode/skills/
cp -r 知识库构建器/skills/dev-extract .opencode/skills/
```

加载后，OpenCode 会自动发现并加载这些 Skill。

### 方式 B：全局加载

将 Skill 目录复制到 OpenCode 全局配置目录，这样所有项目都可以使用。

#### 图形界面操作（Windows）

1. 按 `Win + R` 打开运行窗口
2. 输入 `%USERPROFILE%\.config\opencode\skills` 并回车
   - 如果提示"找不到文件"，说明目录不存在，需要手动创建
3. 手动创建步骤：
   - 打开「此电脑」→ 进入 `C:\Users\你的用户名\`
   - 依次进入 `.config` → `opencode` 文件夹
   - 如果没有 `.config` 文件夹 → 新建文件夹命名为 `.config`
   - 在 `opencode` 文件夹中新建 `skills` 文件夹
4. 回到「知识库构建器」的 `skills` 文件夹，复制 `knowledge-base-builder` 和 `dev-extract`
5. 粘贴到刚才打开的全局 `skills` 文件夹中

#### 图形界面操作（macOS/Linux）

1. 打开 Finder（macOS）或文件管理器（Linux）
2. 按 `Cmd + Shift + G`（macOS）或 `Ctrl + L`（Linux）
3. 输入 `~/.config/opencode/skills` 并回车
   - 如果目录不存在，手动创建：`~/.config/opencode/skills`
4. 复制「知识库构建器」的 `skills` 文件夹中的内容到此目录

#### 命令行操作（可选，技术用户）

```bash
# Linux/macOS
cp -r 知识库构建器/skills/knowledge-base-builder ~/.config/opencode/skills/
cp -r 知识库构建器/skills/dev-extract ~/.config/opencode/skills/

# Windows PowerShell
Copy-Item -Recurse "知识库构建器\skills\knowledge-base-builder" "$env:USERPROFILE\.config\opencode\skills\"
Copy-Item -Recurse "知识库构建器\skills\dev-extract" "$env:USERPROFILE\.config\opencode\skills\"
```

全局加载后，所有 OpenCode 项目都可以使用这些 Skill。

---

## 8. 配置文件

### opencode.json 配置示例

在项目根目录创建或编辑 `opencode.json`：

#### 图形界面操作

1. 打开你的项目文件夹
2. 查找是否已有 `opencode.json` 文件
3. 如果有 → 右键 → **打开方式** → 选择记事本（Windows）或文本编辑（macOS）
4. 如果没有 → 右键 → **新建** → **文本文档** → 重命名为 `opencode.json`（注意扩展名必须是 `.json`）
5. 打开文件，粘贴以下内容：

```json
{
  "skills": [
    "./skills/knowledge-base-builder",
    "./skills/dev-extract"
  ]
}
```

6. 保存并关闭

**如果使用全局加载（方式 B），则不需要配置此文件**。

---

## 9. 使用方式

### 手动触发

在对话中直接提及 Skill 名称：

```
用户: 使用 knowledge-base-builder 初始化我的知识库
用户: 使用 dev-extract 提炼本次对话的知识点
```

### 自动触发

- **knowledge-base-builder**: 当用户提到「初始化知识库」「构建知识库」「整理资料」等关键词时自动触发
- **dev-extract**: 当用户说「记录一下」「提炼」「extract」「存档」等关键词时自动触发，或 session 结束时自动扫描

---

## 10. 权限配置

确保 OpenCode 有足够的文件系统权限：

- 读取 `原始资料/` 目录
- 写入 `wiki/` 目录
- 创建和编辑 Markdown 文件

OpenCode 默认具有当前工作目录的读写权限，通常无需额外配置。

---

## 11. 注意事项

- OpenCode 版本要求：建议使用最新版本以确保 Skill 功能完整
- Skill 文件必须包含 `SKILL.md` 作为入口文件
- 如果 Skill 未被自动发现，检查 `.opencode/skills/` 目录结构是否正确
- **隐藏文件夹提示**：以 `.` 开头的文件夹（如 `.opencode`）在 Windows/macOS 中默认隐藏，需要开启"显示隐藏文件"才能看到
  - Windows：在文件资源管理器中点击「查看」→ 勾选「隐藏的项目」
  - macOS：按 `Cmd + Shift + .`

---

## 12. 推荐插件

### 🎯 oh-my-openagent（omo）— 首推

**一句话**：将 OpenCode 从单 Agent 升级为多模型多 Agent 协作团队。

**功能**：多模型 Agent 编排工具，核心 Agent Sisyphus 可调度 Oracle（架构）、Librarian（文档）、Explore（代码搜索）等子 Agent 并行协作。支持 Team Mode 同时运行 Claude、GPT、Kimi 等多种模型。提供 `ultrawork` / `/ulw-loop` 一键持续工作模式。

**安装**：在 OpenCode 对话中发送：
```
Install and configure oh-my-openagent by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

### ⚡ GSD（get-shit-done）— 强力推荐

**一句话**：轻量级元提示与上下文工程系统，专治长会话质量劣化。

**功能**：解决 AI 编码助手最常见的 context rot 问题。一键安装即可使用，提供阶段化管理、自动验证等工作流。已被 Amazon、Google、Shopify 工程师采用。

**安装**：
```bash
npx get-shit-done-cc@latest
```

### 🔗 更多插件

> superpowers（完整开发方法论）、graphify（知识图谱）、claude-mem（跨会话记忆）、spec-kit（规格驱动开发）、Context7（最新文档查询）等。
>
> 📖 完整对比与搭配建议详见 [插件与工具推荐.md](./插件与工具推荐.md)。

---

## 13. 国内用户提示

> 💡 **国内用户遇到问题？**
>
> 如果你在国内使用 OpenCode，可能遇到网络访问、模型接入、npm 下载等问题。
> 完整的解决方案（npm 镜像源、代理配置、国内模型推荐、Ollama 本地部署等）详见 [常见问题与解决方案.md](./常见问题与解决方案.md)。
