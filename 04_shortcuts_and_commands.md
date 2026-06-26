# 04 · 主要快捷键、命令与使用方法

> **系列索引**：[README](./README.md) · [01 终端配置](./01_terminal_setup.md) · [02 安装 CC](./02_installation.md) · [03 唤起与使用](./03_launch_and_use.md) · **本篇** · [05 目录结构](./05_directory_structure.md)

---

## 前言

本文是 Claude Code 的**速查手册**，涵盖：交互快捷键、权限模式、斜杠命令、CLI flags、管道模式、Skills 安装使用，以及实用技巧。建议收藏备查。

> 内网通过 `mc --code` 启动，本文所有 `claude` 命令均对应内网的 `mc --code` 入口，flags 写法完全一致。

---

## 目录

- [一、交互界面快捷键](#一交互界面快捷键)
- [二、权限模式速查](#二权限模式速查)
- [三、斜杠命令](#三斜杠命令commands)
- [四、CLI 启动命令](#四cli-启动命令)
- [五、常用 CLI Flags](#五常用-cli-flags)
- [六、管道与非交互模式](#六管道与非交互模式)
- [七、Skills 的安装与使用](#七skills-技能的安装与使用)
- [八、会话管理](#八会话管理)
- [九、实用技巧](#九实用技巧)

---

## 一、交互界面快捷键

在 Claude Code 交互界面中，以下快捷键可直接使用：

| 快捷键 | 功能 |
|--------|------|
| `Enter` | 发送消息 |
| `Shift+Enter` | 消息内换行（不发送） |
| `↑` / `↓` | 浏览历史消息 |
| `Esc` | 中断当前正在执行的操作 |
| `Ctrl+C` | 退出 Claude Code |
| `Ctrl+L` | 清屏（保留上下文） |
| `Tab` | 循环切换权限模式（default → acceptEdits → plan → auto → bypassPermissions） |
| `Shift+Tab` | 反向切换权限模式 |
| `@` | 引用文件（弹出文件选择器） |
| `#` | 添加记忆（Memory） |
| `!` | 直接执行 shell 命令（不经过 Claude） |
| `/` | 输入斜杠命令 |

---

## 二、权限模式速查

按 `Tab` 循环切换，当前模式显示在提示符旁：

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| `default` | 默认模式，危险操作需手动确认 | 新手或不熟悉的项目 |
| `acceptEdits` | 自动接受文件编辑，其余操作仍需确认 | 以写代码为主的任务 |
| `plan` | 只规划，不执行任何操作 | 查看 Claude 的执行计划后再决定 |
| `auto` | 自动模式，根据规则判断是否需要确认 | 半自动化场景 |
| `bypassPermissions` | 跳过所有权限检查（即 `--dangerously-skip-permissions`） | 熟悉后的日常主力模式 |

---

## 三、斜杠命令（/commands）

在对话框中输入 `/` 触发命令列表：

### 基础命令

| 命令 | 功能 |
|------|------|
| `/help` | 查看所有可用命令 |
| `/clear` | 清空当前对话上下文，开始新任务 |
| `/compact` | 压缩上下文（保留摘要，释放 token） |
| `/status` | 查看当前会话状态、配置来源、token 用量 |

### 会话管理

| 命令 | 功能 |
|------|------|
| `/resume` | 恢复历史会话（交互式选择） |
| `/rename <名称>` | 给当前会话命名，方便后续 resume |

### 模型与配置

| 命令 | 功能 |
|------|------|
| `/model` | 切换模型 |
| `/permissions` | 查看当前权限规则 |
| `/memory` | 查看和编辑 Memory 内容 |

### 项目工具

| 命令 | 功能 |
|------|------|
| `/init` | 初始化 CLAUDE.md（扫描项目自动生成） |
| `/review` | 对当前 PR 进行代码审查 |
| `/pr-comments` | 查看当前 PR 的评论 |

### 其他

| 命令 | 功能 |
|------|------|
| `/vim` | 切换 Vim 输入模式 |
| `/terminal-setup` | 配置终端 shell 集成（Shift+Enter 换行等） |
| `/bug` | 向 Anthropic 报告问题 |
| `/doctor` | 检查 Claude Code 安装状态 |

---

## 四、CLI 启动命令

内网用 `mc --code [flags]` 代替 `claude [flags]`，flags 完全一致：

| 命令 | 说明 | 示例 |
|------|------|------|
| `mc --code` | 启动交互会话 | `mc --code` |
| `mc --code "问题"` | 携带初始提示启动 | `mc --code "解释这个项目"` |
| `mc --code -p "问题"` | 非交互模式，输出后退出 | `mc --code -p "这个函数做什么"` |
| `mc --code -c` | 继续当前目录最近的会话 | `mc --code -c` |
| `mc --code -r "会话名"` | 恢复指定名称/ID 的会话 | `mc --code -r "auth-refactor"` |
| `mc --code -n "名称"` | 为本次会话命名 | `mc --code -n "feature-login"` |

---

## 五、常用 CLI Flags

| Flag | 说明 | 示例 |
|------|------|------|
| `--dangerously-skip-permissions` | 跳过所有权限确认（即 alias `cc`） | `mc --code --dangerously-skip-permissions` |
| `--permission-mode plan` | 以 plan 模式启动（只规划不执行） | `mc --code --permission-mode plan` |
| `--model <model-id>` | 指定模型 | `mc --code --model claude-sonnet-4-6` |
| `--effort <level>` | 设置思考力度（low / medium / high / xhigh / max） | `mc --code --effort high` |
| `--add-dir <路径>` | 额外授权访问的目录 | `mc --code --add-dir ../shared` |
| `--append-system-prompt "..."` | 追加自定义系统提示 | `mc --code --append-system-prompt "始终用中文回复"` |
| `-v` | 查看版本号 | `mc --code -v` |
| `--verbose` | 详细日志模式（调试用） | `mc --code --verbose` |
| `--worktree <名称>` | 在独立 git worktree 中启动 | `mc --code -w feature-auth` |

---

## 六、管道与非交互模式

`-p` 模式让 Claude Code 像命令行工具一样使用，适合脚本集成和批量处理：

```bash
# 分析错误日志
cat error.log | mc --code -p "分析报错原因并给出修复建议"

# 生成 git commit message
git diff --staged | mc --code -p "为这些改动生成一个规范的 commit message"

# 代码安全审查
cat src/auth.js | mc --code -p "审查这段代码的安全性，重点关注注入和越权问题"

# 输出 JSON 格式（适合脚本解析）
mc --code -p "列出项目的主要依赖及其用途" --output-format json

# 批量处理文件
for f in src/**/*.ts; do
  echo "=== $f ===" >> review.md
  cat "$f" | mc --code -p "简要说明这个文件的职责" >> review.md
done
```

---

## 七、Skills（技能）的安装与使用

Skills 是可安装的扩展命令包，安装后通过 `/skill名称` 调用。

### 查看已安装的 Skills

在 Claude Code 交互界面中执行 `/help`，所有以 `/` 开头的自定义命令即为已安装的 Skills。

### 安装 Skill

```bash
# 方式一：在 Claude Code 中直接安装（推荐）
> 帮我安装 <skill名称> skill

# 方式二：通过 claude plugin 命令
claude plugin install <skill名称>@<marketplace>

# 示例：安装官方 code-review skill
claude plugin install code-review@claude-plugins-official
```

### 使用 Skill

安装后直接在对话中输入斜杠命令触发：

```
/review           # 代码审查
/init             # 初始化项目 CLAUDE.md
/security-review  # 安全审查
```

### 自定义 Skill

Skills 本质上是放在 `~/.claude/skills/` 或 `.claude/skills/` 目录下的 Markdown 文件，文件名即命令名：

```bash
# 查看已有 skill 文件
ls ~/.claude/skills/

# 新建一个自定义 skill
cat > ~/.claude/skills/my-review.md << 'EOF'
---
description: 对当前改动进行代码审查（安全、性能、可读性）
---

# 代码审查

对当前改动进行审查，重点关注：
1. 安全漏洞（SQL 注入、XSS、CSRF 等）
2. 性能问题（N+1 查询、不必要的循环等）
3. 代码可读性和命名规范
4. 错误处理是否完整

输出格式：按严重程度（高/中/低）分级列出问题，每个问题附上修改建议。
EOF
```

---

## 八、会话管理

```bash
# 为本次会话命名（启动时）
mc --code -n "feature-payment"

# 恢复指定名称/ID 的会话
mc --code -r "feature-payment"

# 继续当前目录最近的会话
mc --code -c

# 在会话中途重命名
/rename new-name

# 交互式选择并恢复历史会话
/resume
```

> 💡 **建议**：对于持续时间较长的任务，启动时用 `-n` 命名会话，方便下次快速恢复，不必从头开始。

---

## 九、实用技巧

### @ 引用文件

在对话中输入 `@` 会弹出文件选择器，可快速引用当前项目的文件，Claude Code 会直接读取并理解该文件：

```
> 帮我重构 @src/utils/auth.ts，提取公共逻辑
> 对比 @api/old.js 和 @api/new.js 的差异，指出潜在问题
```

### ! 执行 Shell 命令

在对话框输入 `!` 前缀可直接执行 shell 命令，结果会显示在界面中，Claude Code 可以直接引用输出：

```bash
! git status          # 查看 git 状态
! npm run test        # 跑测试
! ls -la src/         # 查看目录
! cat package.json    # 查看依赖配置
```

### # 添加持久记忆

输入 `#` 可以向 Claude Code 的 Memory 添加持久记忆，下次启动仍然有效，无需重复说明背景：

```
# 这个项目使用 pnpm 而不是 npm
# 数据库密码在 .env.local 中，不要读取或输出
# 代码审查时重点关注 TypeScript 类型安全
# 提交信息用英文，遵循 Conventional Commits 规范
```

### 多任务并行

配合 tmux 或 Ghostty 分屏，可以同时运行多个 Claude Code 实例，分别处理不同的任务：

```bash
# 分屏 1：处理前端重构
cd ~/project/frontend && cc

# 分屏 2：处理后端 API
cd ~/project/backend && cc
```

---

> ← [03 · 唤起与使用](./03_launch_and_use.md) | **下一篇**：[05 · 目录结构 →](./05_directory_structure.md)
