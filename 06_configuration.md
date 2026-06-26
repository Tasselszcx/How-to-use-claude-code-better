# 06 · 把 Claude Code 调教成你想要的样子 — 配置全解

> **系列索引**：[README](./README.md) · [01](./01_terminal_setup.md) · [02](./02_installation.md) · [03](./03_launch_and_use.md) · [04](./04_shortcuts_and_commands.md) · [05](./05_directory_structure.md) · **本篇** · [07](./07_ask_user_question.md) · [08](./08_claude_md_in_practice.md) · [09](./09_hooks_in_practice.md) · [10](./10_multi_agent_teams.md) · [11](./11_code_review.md) · [12](./12_effective_prompting.md)

---

## 前言

合理的配置能大幅提升 Claude Code 的使用效率。本文介绍三个最重要的配置入口：CLAUDE.md、settings.json / settings.local.json，以及几个能立竿见影的提效配置。

---

## 目录

- [一、CLAUDE.md：给 Claude Code 的"项目说明书"](#一claudemd给-claude-code-的项目说明书)
- [二、settings.json：行为控制](#二settingsjson行为控制)
- [三、权限配置详解](#三权限配置详解)
- [四、提效配置推荐](#四提效配置推荐)
- [五、完整配置示例](#五完整配置示例)

---

## 一、CLAUDE.md：给 Claude Code 的"项目说明书"

CLAUDE.md 是最重要的配置文件。每次启动 Claude Code，它都会自动加载这个文件，相当于给 Claude 的持久化系统提示。

### 写什么

好的 CLAUDE.md 应该包含 Claude Code **自己读不到**的信息：

```markdown
# 项目说明

## 技术栈
- 前端：React 18 + TypeScript + Vite
- 后端：Node.js + Express + PostgreSQL
- 包管理：pnpm（不要用 npm/yarn）

## 常用命令
- 启动开发服务器：`pnpm dev`
- 运行测试：`pnpm test`
- 代码检查：`pnpm lint`
- 构建：`pnpm build`

## 代码规范
- 使用 2 空格缩进
- 组件文件用 PascalCase，工具函数用 camelCase
- 所有函数必须有 TypeScript 类型注解
- 禁止使用 `any` 类型

## 注意事项
- 数据库迁移文件不要手动修改
- .env 文件不要提交，使用 .env.example 作为模板
- 生产环境配置在 config/prod.ts，不要直接修改
```

### 快速生成 CLAUDE.md

```bash
# 在 Claude Code 中执行，会自动扫描项目生成初稿
/init
```

### 配置层级

| 文件 | 作用范围 | 是否提交 git |
|------|---------|-------------|
| `~/.claude/CLAUDE.md` | 所有项目（全局生效） | — |
| `<项目>/CLAUDE.md` | 当前项目（团队共享） | ✓ |
| `<项目>/CLAUDE.local.md` | 当前项目（个人私有） | ✗ |

---

## 二、settings.json：行为控制

### 文件位置与优先级

| 文件 | 作用范围 | 是否提交 git |
|------|---------|-------------|
| `~/.claude/settings.json` | 所有项目（用户级） | — |
| `<项目>/.claude/settings.json` | 当前项目（团队共享） | ✓ |
| `<项目>/.claude/settings.local.json` | 当前项目（个人私有） | ✗ |

**优先级**：本地 > 项目 > 用户，数组类配置（如权限规则）会合并而非覆盖。

### 推荐的用户级配置（`~/.claude/settings.json`）

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "claude-sonnet-4-6",
  "language": "chinese",
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm run *)",
      "Bash(pnpm *)",
      "Bash(ls *)",
      "Bash(cat *)",
      "Bash(grep *)",
      "Read(**)"
    ]
  }
}
```

### 推荐的项目级配置（`.claude/settings.json`）

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": [
      "Bash(pnpm run *)",
      "Bash(pnpm test *)",
      "Bash(pnpm lint)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./secrets/**)"
    ]
  }
}
```

---

## 三、权限配置详解

权限配置控制哪些操作无需弹窗确认。

### 权限规则语法

```json
{
  "permissions": {
    "allow": [
      "Bash(git log *)",            // git log 及任意参数
      "Bash(npm run test *)",       // npm test 及参数
      "Read(./src/**)",             // 读取 src 目录下所有文件
      "Edit(./src/**)",             // 编辑 src 目录下所有文件
      "WebFetch(domain:github.com)" // 访问 github.com
    ],
    "deny": [
      "Bash(rm -rf *)",             // 拒绝危险删除
      "Read(./.env)",               // 拒绝读取 .env
      "Read(./secrets/**)"          // 拒绝读取 secrets 目录
    ]
  }
}
```

### 快速减少权限弹窗

对于已经信任的操作，加到 allow 列表即可不再弹窗：

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(pnpm *)",
      "Bash(node *)",
      "Bash(ls *)",
      "Bash(cat *)",
      "Bash(find *)",
      "Bash(grep *)",
      "Bash(echo *)",
      "Read(**)",
      "WebFetch"
    ]
  }
}
```

---

## 四、提效配置推荐

### 1. 设置默认权限模式

如果你大多数时候都用 yolo 模式，可以在用户级配置中设置默认：

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

等效于每次启动都加 `--dangerously-skip-permissions`，但更灵活——可以在项目级覆盖为更严格的模式。

### 2. 锁定回复语言

```json
{
  "language": "chinese"
}
```

防止 Claude Code 在某些情况下切换回英文回复。

### 3. 设置思考深度

```json
{
  "effortLevel": "high"
}
```

可选值：`low` / `medium` / `high` / `xhigh` / `max`。对于复杂任务，`high` 或 `xhigh` 效果更好，但会消耗更多 token。

### 4. 配置 Hooks（自动化触发器）

Hooks 可以在特定事件发生时自动执行命令，比如任务完成时发桌面通知：

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code 完成了任务\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

常用 Hook 事件：

| 事件 | 触发时机 |
|------|---------|
| `PreToolUse` | 工具调用前 |
| `PostToolUse` | 工具调用后 |
| `Stop` | Claude Code 完成任务停止时 |
| `Notification` | 需要用户注意时 |

> 详细的 Hooks 用法见 [09 · Hooks 实战](./09_hooks_in_practice.md)。

### 5. 用 /status 验证配置是否生效

修改配置后，在 Claude Code 中执行 `/status`，会显示当前所有配置的来源和值，方便确认是否正确加载。

---

## 五、完整配置示例

### 个人全局配置（`~/.claude/settings.json`）

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "claude-sonnet-4-6",
  "language": "chinese",
  "effortLevel": "high",
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(pnpm *)",
      "Bash(ls *)",
      "Bash(cat *)",
      "Bash(grep *)",
      "Read(**)",
      "WebFetch"
    ]
  },
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"任务完成\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

### 个人全局指令（`~/.claude/CLAUDE.md`）

```markdown
# 全局工作偏好

## 回复风格
- 始终用中文回复
- 代码修改要简洁，不引入不必要的抽象
- 修改前先说明方案，不要直接动手

## 代码习惯
- 优先使用已有的工具和库，不随意引入新依赖
- 不写无意义的注释，代码本身要自解释
- 错误处理只在真正需要的地方加

## 安全习惯
- 不提交 .env 文件
- 不在代码里硬编码密码或 token
```

---

> ← [05 · 目录结构](./05_directory_structure.md) | **下一篇**：[07 · AskUserQuestion →](./07_ask_user_question.md)
