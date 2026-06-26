# 03 · 唤起并使用 Claude Code

> **系列索引**：[README](./README.md) · [01 终端配置](./01_terminal_setup.md) · [02 安装 CC](./02_installation.md) · **本篇** · [04 命令速查](./04_shortcuts_and_commands.md) · [05 目录结构](./05_directory_structure.md)

---

## 前言

装好 Claude Code 之后，第一个问题就是：**在哪里、怎么打开它？**

Claude Code 本质上是一个运行在终端里的交互式 AI Agent，它的「感知范围」就是你启动时所在的目录——它会读取该目录下的文件、代码、Git 历史，然后根据你的对话来操作。

> 💡 **核心理念**：**在哪个目录启动，就等于告诉它在哪个项目里工作。**

本文介绍 4 种主要的唤起方式。

---

## 目录

- [方式一：终端直接唤起（最常用）](#方式一终端直接唤起最常用)
- [方式二：在 VS Code 中使用](#方式二在-vs-code-中使用)
- [方式三：Ghostty + VS Code 联动](#方式三ghostty--vs-code-联动)
- [方式四：Quick Terminal 随时呼出](#方式四quick-terminal-随时呼出)
- [启动后的基本交互](#启动后的基本交互)

---

## 方式一：终端直接唤起（最常用）

这是最基础也最通用的方式，适合所有项目。

### 操作步骤

```bash
# 1. 进入你的项目目录
cd ~/your-project

# 2. 启动 Claude Code
cc
```

### Claude Code 能感知什么

启动后，Claude Code 会自动了解当前目录的：

| 感知内容 | 说明 |
|---------|------|
| 文件结构 | 目录树，文件名和层级关系 |
| 代码内容 | 按需读取，不会一次性全量加载 |
| Git 信息 | 历史记录、当前分支、未提交的改动 |
| 项目配置 | `package.json`、`CLAUDE.md`、`.env` 等 |

你可以直接提问，无需手动粘贴代码：

```
> 帮我看看这个项目的整体结构
> 找一下处理用户登录的逻辑在哪里
> 给 getUserById 函数加上错误处理
> 帮我写这个功能的单元测试
```

### 推荐：用 cca 在固定工作目录启动

如果你有一个专门做实验或写脚本的目录（比如 `~/claude/`），可以用 `cca` 一键切过去并启动：

```bash
cca   # 等价于：cd ~/claude/ && mc --code --dangerously-skip-permissions
```

---

## 方式二：在 VS Code 中使用

Claude Code 有官方的 VS Code 插件，可以让它直接感知你当前打开的文件和选中的代码，体验更流畅。

### 安装插件

在 VS Code 扩展市场搜索 **Claude Code**（发布者：Anthropic），或访问：

🔗 [marketplace.visualstudio.com/items?itemName=anthropic.claude-code](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code)

> 要求 VS Code 版本 ≥ 1.98.0

### 内网使用说明

> ⚠️ **重要**：VS Code 插件默认通过 claude.ai 账号认证。在美团内网，需要通过 CatPaw CLI 代理启动，**不要直接使用插件的登录入口**。

正确的内网使用方式：在 VS Code 的**集成终端**里执行 `cc` 启动 Claude Code：

```bash
# 在 VS Code 集成终端（Ctrl+` 打开）中执行
cc
```

这样既能享受终端交互，又能让 Claude Code 感知当前 VS Code 打开的工作区。

### 插件的额外能力

安装插件后，Claude Code 在终端运行时额外获得：

| 能力 | 说明 |
|------|------|
| 感知当前打开的文件 | Claude Code 知道你在编辑器里看的是哪个文件 |
| 感知选中的代码 | 你在编辑器里选中一段代码，Claude Code 可以直接引用 |
| 内联差异预览 | 修改文件时，改动以 diff 形式显示在编辑器里，可逐块接受或拒绝 |

---

## 方式三：Ghostty + VS Code 联动

这是体验最完整的方式：用 Ghostty 的分屏功能，左边跑 Claude Code，右边开 VS Code，两边实时联动。

### 操作步骤

```bash
# 步骤 1：在 Ghostty 左侧面板启动 Claude Code
cd ~/your-project && cc

# 步骤 2：按 Cmd+D 向右分屏
# 步骤 3：在右侧面板打开 VS Code
code .
```

VS Code 打开后，安装 Claude Code 插件（参见方式二），即可实现双向联动。

### 三种使用方式对比

| 维度 | 纯终端 | 纯 VS Code | Ghostty + VS Code |
|------|--------|-----------|------------------|
| Claude Code 交互 | ✅ | ✅（集成终端） | ✅（独立面板，空间更大） |
| 感知编辑器状态 | ❌ | ✅ | ✅ |
| 同时看代码和对话 | 需手动分屏 | 需拖动布局 | 天然分屏，互不干扰 |
| Quick Terminal 随时呼出 | ✅ | ❌ | ✅ |
| 推荐场景 | 快速任务 | 已有 VS Code 习惯 | 主力工作流 |

> 💡 **小技巧**：Ghostty 的 `Cmd+Space` Quick Terminal 可以在任意应用（包括 VS Code 全屏时）呼出，临时问 Claude Code 一个问题后收起，完全不打断编码节奏。

---

## 方式四：Quick Terminal 随时呼出

如果你不想专门开一个终端窗口，Ghostty 的 Quick Terminal 是最轻量的方式。

**操作步骤：**

1. 在任意应用中按 `Cmd+Space` → Ghostty 从屏幕顶部滑出
2. 如果还没启动 Claude Code，执行 `cc`
3. 问完问题后再按 `Cmd+Space` 收起，回到之前的工作

**适合场景：**
- 在浏览器查资料时顺手问一句
- 在 Slack 讨论需求时快速验证一个想法
- 任何需要「问完即走」的临时查询

---

## 启动后的基本交互

Claude Code 启动后是一个持续的对话界面，常用操作如下：

| 操作 | 说明 |
|------|------|
| 直接输入问题 | 提问或下达任务，按 `Enter` 发送 |
| `Shift+Enter` | 消息内换行（不发送） |
| `Esc` | 中断当前正在执行的操作 |
| `/clear` | 清空对话上下文，开始新任务 |
| `/compact` | 压缩上下文（保留摘要，节省 token） |
| `/help` | 查看所有可用命令 |
| `Ctrl+C` | 退出 Claude Code |
| `@文件名` | 在对话中引用项目文件 |
| `!命令` | 直接执行 shell 命令 |

### 第一次使用建议

```
> 你好，简单介绍一下这个项目的结构
> 帮我找到入口文件在哪里
> 有什么可以改进的地方吗
```

从简单的探索性问题开始，让 Claude Code 先熟悉你的项目，再逐步交付更复杂的任务。

---

> ← [02 · 安装 CC](./02_installation.md) | **下一篇**：[04 · 命令速查 →](./04_shortcuts_and_commands.md)
