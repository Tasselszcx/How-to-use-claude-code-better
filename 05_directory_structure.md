# 05 · CC 的主要目录结构

> **系列索引**：[README](./README.md) · [01 终端配置](./01_terminal_setup.md) · [02 安装 CC](./02_installation.md) · [03 唤起与使用](./03_launch_and_use.md) · [04 命令速查](./04_shortcuts_and_commands.md) · **本篇**

---

## 前言

了解 Claude Code 把数据存在哪里，能帮你：
- 更好地管理配置和自定义指令
- 快速定位并恢复历史会话
- 在多台机器间同步设置
- 排查配置不生效等常见问题

本文梳理 Claude Code 的完整目录结构和各文件的作用。

---

## 目录

- [一、全局目录 `~/.claude/`](#一全局目录claude)
- [二、项目目录 `.claude/`](#二项目目录claude)
- [三、CLAUDE.md 加载机制](#三claudemd-加载机制)
- [四、会话文件结构](#四会话文件结构)
- [五、Skills 文件结构](#五skills-文件结构)
- [六、快速定位常用文件](#六快速定位常用文件)

---

## 一、全局目录 `~/.claude/`

这是 Claude Code 最重要的目录，存放所有**用户级**的配置和数据（对所有项目生效）：

```
~/.claude/
├── settings.json          # 用户级全局配置（对所有项目生效）
├── CLAUDE.md              # 用户级全局指令（所有项目启动时自动加载）
├── CLAUDE.local.md        # 用户级本地指令（不提交到 git）
│
├── skills/                # 全局 Skills 目录
│   ├── review.md          # → /review 命令
│   ├── security.md        # → /security 命令
│   └── ...
│
├── memory/                # 自动记忆存储目录
│   ├── MEMORY.md          # 记忆索引文件（被加载到上下文中）
│   └── *.md               # 各类具体记忆文件
│
├── projects/              # 会话历史存储（按项目路径分组）
│   └── <路径hash>/
│       └── <uuid>.jsonl   # 每个会话一个 JSONL 文件
│
├── plans/                 # 计划文件目录
├── todos/                 # 任务列表存储
├── keybindings.json       # 自定义快捷键绑定
├── statsig/               # 功能开关缓存（无需手动修改）
└── .credentials.json      # 认证凭据（⚠️ 敏感文件，勿提交）
```

### 关键文件详解

#### `settings.json` — 全局配置

对所有项目生效。常用配置示例：

```json
{
  "model": "claude-sonnet-4-6",
  "language": "chinese",
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git *)",
      "Bash(ls *)"
    ],
    "deny": [
      "Bash(rm -rf *)"
    ]
  }
}
```

#### `CLAUDE.md` — 全局指令

每次启动 Claude Code 都会加载。适合写：
- 你的工作习惯和偏好（如「始终用中文回复」）
- 通用的代码风格要求
- 常用工具说明（如「包管理器用 pnpm，不用 npm」）
- 不想 Claude 做的事情（如「不要直接修改生产配置文件」）

#### `memory/` — 持久记忆

通过 `#` 命令或 AI 自动写入的记忆系统。`MEMORY.md` 是索引，其余 `.md` 文件是具体记忆内容。记忆在每次启动时自动加载，无需重复说明背景。

#### `projects/` — 会话历史

所有会话历史按项目路径的 hash 分组存储。每个 `.jsonl` 文件是一个完整的会话记录，包含所有对话内容、工具调用和 token 用量。

通过 `mc --code -r` 或 `/resume` 可以从这里恢复历史会话。

#### `.credentials.json` — 认证凭据

⚠️ **敏感文件**，包含登录令牌和 API Key，切勿提交到 git 或分享给他人。

---

## 二、项目目录 `.claude/`

在每个项目根目录下，Claude Code 会识别 `.claude/` 目录：

```
your-project/
├── .claude/
│   ├── settings.json        # 项目级配置（建议提交到 git，团队共享）
│   ├── settings.local.json  # 本地覆盖配置（加入 .gitignore，不提交）
│   ├── skills/              # 项目级 Skills（仅在此项目可用）
│   │   └── project-skill.md
│   ├── plans/               # 计划文件（可选是否提交）
│   └── worktrees/           # git worktree 存储目录（不提交）
│
├── CLAUDE.md                # 项目级指令（建议提交到 git）
├── CLAUDE.local.md          # 项目级本地指令（不提交）
└── ...（项目文件）
```

### 配置加载优先级

Claude Code 启动时，按以下顺序加载配置，**后者覆盖前者**：

```
用户级 (~/.claude/settings.json)
    ↓
项目级 (.claude/settings.json)
    ↓
本地覆盖 (.claude/settings.local.json)
```

> 💡 **实用场景**：团队统一的项目规范放在 `.claude/settings.json`（提交到 git），个人偏好放在 `.claude/settings.local.json`（加入 .gitignore），互不干扰。

### `.gitignore` 推荐配置

在项目 `.gitignore` 中添加：

```gitignore
# Claude Code 本地配置（含个人信息，不提交）
.claude/settings.local.json
CLAUDE.local.md

# worktree 目录（本地生成，不提交）
.claude/worktrees/
```

---

## 三、CLAUDE.md 加载机制

Claude Code 启动时会从多个位置加载 CLAUDE.md，并按顺序合并：

```
加载顺序（数字越小越先加载，后加载的可覆盖前者）：

1. ~/.claude/CLAUDE.md            ← 用户全局指令（最先加载）
2. 项目根目录/CLAUDE.md            ← 项目主指令
3. 项目根目录/.claude/CLAUDE.md   ← 项目 .claude 目录下的指令
4. 父级目录/CLAUDE.md             ← 逐级向上查找，直到根目录
5. CLAUDE.local.md                ← 本地覆盖指令（最后加载）
```

### Monorepo 场景

在 monorepo 中，可以在每个子包目录放一个 `CLAUDE.md`，Claude Code 进入该目录时会自动加载对应的指令：

```
monorepo/
├── CLAUDE.md              # 顶层通用指令
├── packages/
│   ├── frontend/
│   │   ├── CLAUDE.md      # 前端专属指令（React/CSS 规范等）
│   │   └── ...
│   └── backend/
│       ├── CLAUDE.md      # 后端专属指令（API 规范、数据库等）
│       └── ...
```

### CLAUDE.md 内容建议

```markdown
# 项目说明

这是一个 [项目类型] 项目，主要功能是 [XXX]。

## 技术栈
- 语言：TypeScript
- 包管理：pnpm（不要用 npm 或 yarn）
- 测试：Vitest
- 数据库：PostgreSQL（ORM：Drizzle）

## 代码规范
- 提交信息使用英文，遵循 Conventional Commits
- 所有导出函数必须有 JSDoc 注释
- 避免使用 any 类型

## 注意事项
- 不要修改 .env.production 文件
- 数据库 Migration 需要人工确认后再执行
```

---

## 四、会话文件结构

每个会话存储为 JSONL 格式，路径规则：

```
~/.claude/projects/<项目路径的URL编码>/<会话UUID>.jsonl
```

每行是一条记录，包含：

| 字段 | 内容 |
|------|------|
| 用户消息 | 你输入的内容 |
| Claude 回复 | Claude 的完整输出 |
| 工具调用 | 读文件、执行命令等操作及其结果 |
| 元信息 | 时间戳、token 用量、会话 ID 等 |

> 💡 会话文件是纯文本 JSONL，可以用 `jq` 等工具解析和搜索历史记录。

---

## 五、Skills 文件结构

Skills 是 Markdown 文件，Claude Code 将其作为斜杠命令加载：

```
~/.claude/skills/
    review.md         → /review
    security.md       → /security
    my-workflow.md    → /my-workflow
```

Skill 文件的标准结构：

```markdown
---
description: 对当前改动进行代码审查
---

# 代码审查

检查以下维度：

1. **安全漏洞**：SQL 注入、XSS、CSRF、越权访问
2. **性能问题**：N+1 查询、不必要的循环、内存泄漏
3. **错误处理**：异常捕获是否完整，错误信息是否安全
4. **代码可读性**：命名是否清晰，注释是否必要

输出格式：
- 按严重程度（高/中/低）分级列出问题
- 每个问题附上：位置、描述、修改建议
- 最后给出整体评分（1-10）
```

---

## 六、快速定位常用文件

| 需求 | 文件路径 |
|------|----------|
| 修改全局配置 | `~/.claude/settings.json` |
| 写全局通用指令 | `~/.claude/CLAUDE.md` |
| 添加全局 Skill | `~/.claude/skills/<name>.md` |
| 查看会话历史 | `~/.claude/projects/` |
| 管理持久记忆 | `~/.claude/memory/MEMORY.md` |
| 修改项目配置（共享） | `<项目>/.claude/settings.json` |
| 修改项目配置（个人）| `<项目>/.claude/settings.local.json` |
| 写项目专属指令 | `<项目>/CLAUDE.md` |
| 自定义快捷键 | `~/.claude/keybindings.json` |

---

> ← [04 · 命令速查](./04_shortcuts_and_commands.md) | [回到 README →](./README.md)
