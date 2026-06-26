# 09 · Hooks 实战 — 让 Claude Code 更自动化

> **系列索引**：[README](./README.md) · [01](./01_terminal_setup.md) · [02](./02_installation.md) · [03](./03_launch_and_use.md) · [04](./04_shortcuts_and_commands.md) · [05](./05_directory_structure.md) · [06](./06_configuration.md) · [07](./07_ask_user_question.md) · [08](./08_claude_md_in_practice.md) · **本篇** · [10](./10_multi_agent_teams.md) · [11](./11_code_review.md) · [12](./12_effective_prompting.md)

---

## 前言

Hooks 是 Claude Code 的自动化触发器：在特定事件发生时（工具调用前后、任务完成时、会话开始时等），自动执行你指定的命令或脚本。

配置好 Hooks，可以实现：任务完成自动通知、每次写文件自动跑 lint、危险操作自动拦截、会话开始自动加载上下文……这些都不需要 Claude Code 记住，而是由系统保证执行。

---

## 目录

- [一、Hooks 的工作原理](#一hooks-的工作原理)
- [二、主要事件类型速查](#二主要事件类型速查)
- [三、最实用的 Hooks 配置](#三最实用的-hooks-配置)
- [四、Hook 输出控制](#四hook-输出控制)
- [五、查看已配置的 Hooks](#五查看已配置的-hooks)
- [六、推荐的 Hooks 组合](#六推荐的-hooks-组合)
- [七、调试 Hooks](#七调试-hooks)

---

## 一、Hooks 的工作原理

Hooks 配置在 `settings.json` 里，基本格式：

```json
{
  "hooks": {
    "事件名": [
      {
        "matcher": "匹配模式",
        "hooks": [
          {
            "type": "command",
            "command": "要执行的命令"
          }
        ]
      }
    ]
  }
}
```

| 字段 | 说明 |
|------|------|
| **事件名** | 什么时候触发（Stop、PreToolUse、PostToolUse 等） |
| **matcher** | 匹配哪个工具（Bash、Edit、Write，或 `*` 匹配所有） |
| **type** | Hook 类型（`command` 最常用） |
| **command** | 要执行的 shell 命令 |

---

## 二、主要事件类型速查

| 事件 | 触发时机 | 常见用途 |
|------|---------|----------|
| `Stop` | Claude Code 完成任务停止时 | 发送完成通知 |
| `PreToolUse` | 工具调用前 | 拦截危险操作、记录日志 |
| `PostToolUse` | 工具调用成功后 | 自动跑 lint / 测试 |
| `SessionStart` | 会话开始或恢复时 | 加载环境、显示项目状态 |
| `Notification` | Claude Code 需要用户注意时 | 自定义通知方式 |
| `UserPromptSubmit` | 用户提交消息前 | 注入上下文、记录日志 |

---

## 三、最实用的 Hooks 配置

### 1. 任务完成时发桌面通知

最高频的需求：Claude Code 跑长任务时你去做别的事，完成了自动通知你。

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code 完成了任务\" with title \"Claude Code\" sound name \"Glass\"'"
          }
        ]
      }
    ]
  }
}
```

配置位置：`~/.claude/settings.json`（全局生效）

### 2. 写文件后自动跑 lint

每次 Claude Code 修改文件，自动执行代码检查，把结果反馈给 Claude Code：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "cd $CLAUDE_PROJECT_DIR && pnpm lint --fix 2>&1 | tail -20"
          }
        ]
      }
    ]
  }
}
```

Claude Code 会看到 lint 输出，如果有问题会自动修复。

### 3. 拦截危险的 shell 命令

阻止 Claude Code 执行 `rm -rf` 等危险操作。

先创建脚本 `.claude/hooks/block-dangerous.sh`：

```bash
#!/bin/bash
INPUT=$(cat)
COMMAND=$(echo "$INPUT" | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('tool_input',{}).get('command',''))" 2>/dev/null)

DANGEROUS_PATTERNS=("rm -rf /" "rm -rf ~" "chmod -R 777 /" "dd if=" "mkfs")

for pattern in "${DANGEROUS_PATTERNS[@]}"; do
  if echo "$COMMAND" | grep -qF "$pattern"; then
    echo '{"decision":"block","reason":"危险命令已被 Hook 拦截：'"$pattern"'"}'
    exit 0
  fi
done

exit 0
```

然后在 `.claude/settings.json` 中配置：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bash $CLAUDE_PROJECT_DIR/.claude/hooks/block-dangerous.sh"
          }
        ]
      }
    ]
  }
}
```

### 4. 会话开始时显示项目状态

每次启动 Claude Code，自动显示 git 状态和最近提交：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo '=== 项目状态 ===' && git status --short && echo '' && echo '=== 最近提交 ===' && git log --oneline -5"
          }
        ]
      }
    ]
  }
}
```

### 5. 写文件后异步跑测试

修改代码后在后台跑测试，不阻塞 Claude Code 继续工作：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "cd $CLAUDE_PROJECT_DIR && pnpm test --passWithNoTests 2>&1 | tail -30",
            "async": true
          }
        ]
      }
    ]
  }
}
```

`async: true` 让测试在后台跑，Claude Code 继续处理下一步。

---

## 四、Hook 输出控制

Hook 脚本通过退出码和 JSON 输出来控制行为：

| 退出码 | 含义 |
|--------|------|
| `0` | 正常，继续执行 |
| `2` | 阻止操作，stdout 内容作为反馈给 Claude Code |
| 其他 | 非阻塞错误，继续执行但显示错误信息 |

阻止操作的示例（在 PreToolUse 中）：

```bash
#!/bin/bash
# 阻止操作，并告诉 Claude Code 原因
echo '{"decision":"block","reason":"此操作被项目规范禁止，请先和我确认"}'
exit 0  # 注意：用 exit 0，通过 JSON 的 decision 字段控制
```

---

## 五、查看已配置的 Hooks

在 Claude Code 中输入：

```
/hooks
```

会打开 Hook 浏览器，显示所有已配置的 Hook、来源（用户 / 项目 / 本地）和命令详情。

---

## 六、推荐的 Hooks 组合

### 个人全局配置（`~/.claude/settings.json`）

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"任务完成\" with title \"Claude Code\" sound name \"Glass\"'"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "git status --short 2>/dev/null && echo '---'"
          }
        ]
      }
    ]
  }
}
```

### 项目级配置（`.claude/settings.json`）

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "cd $CLAUDE_PROJECT_DIR && pnpm lint 2>&1 | tail -10"
          }
        ]
      }
    ]
  }
}
```

---

## 七、调试 Hooks

Hook 不生效时的排查步骤：

```bash
# 1. 检查 JSON 语法是否正确
cat ~/.claude/settings.json | python3 -m json.tool

# 2. 手动执行 Hook 命令验证
bash -c "你的 hook 命令"

# 3. 在 Claude Code 中查看 Hook 配置
/hooks

# 4. 检查脚本权限
chmod +x .claude/hooks/your-script.sh
```

---

> ← [08 · CLAUDE.md 实战](./08_claude_md_in_practice.md) | **下一篇**：[10 · 多 Agent 并行 →](./10_multi_agent_teams.md)
