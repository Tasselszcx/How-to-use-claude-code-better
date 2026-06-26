# 02 · 5 分钟装好 Claude Code 并跑起来

> **系列索引**：[README](./README.md) · [01 终端配置](./01_terminal_setup.md) · **本篇** · [03 唤起与使用](./03_launch_and_use.md) · [04 命令速查](./04_shortcuts_and_commands.md) · [05 目录结构](./05_directory_structure.md)

---

## 前言

上一篇配好了终端环境，这一篇讲 Claude Code 的**安装与启动**。

Claude Code 是 Anthropic 官方出品的命令行 AI 编程助手，直接在终端里与项目代码交互。安装只需一条命令，配好 alias 后两个字母即可启动。

---

## 目录

- [一、安装 Claude Code](#一安装-claude-code)
- [二、登录认证](#二登录认证)
- [三、启动方式](#三启动方式)
- [四、推荐：配置 alias](#四推荐配置-alias)
- [五、三个 alias 的使用场景](#五三个-alias-的使用场景)
- [六、模型说明](#六模型说明)
- [七、快速验证](#七快速验证)
- [常见问题](#常见问题)

---

## 一、安装 Claude Code

需要 Node.js >= 18，两种方式二选一：

```bash
# 方式一：npm 全局安装（推荐）
npm install -g @anthropic-ai/claude-code

# 方式二：官方安装脚本
curl -fsSL https://claude.ai/install.sh | bash
```

安装完成后验证：

```bash
claude --version
```

---

## 二、登录认证

首次使用需要登录你的 Anthropic / Claude.ai 账号：

```bash
claude
```

启动后按提示选择登录方式（浏览器 OAuth 或 API Key），完成后认证信息会保存在 `~/.claude/.credentials.json`，后续启动无需重复登录。

> 💡 如果你有 Anthropic API Key，也可以直接通过环境变量使用：
> ```bash
> export ANTHROPIC_API_KEY="sk-ant-..."
> claude
> ```

---

## 三、启动方式

```bash
# 普通模式：每次操作需要手动确认权限（适合新手或敏感操作）
claude

# Yolo 模式：跳过所有权限确认（适合熟悉后的日常使用）
claude --dangerously-skip-permissions
```

---

## 四、推荐：配置 alias

每次手动输入 `claude --dangerously-skip-permissions` 太繁琐，配置 alias 后只需两个字母即可启动。

在 `~/.zshrc`（或 `~/.bashrc`）末尾追加：

```bash
# Claude Code alias 配置
alias cc='claude --dangerously-skip-permissions'                   # 日常主力：yolo 模式
alias ccb='claude'                                                  # 谨慎模式：需手动确认
alias cca='cd ~/claude/ && claude --dangerously-skip-permissions'  # 在工作目录启动
```

配置后执行以下命令使其立即生效：

```bash
source ~/.zshrc
```

fish 用户在 `~/.config/fish/config.fish` 中使用 `alias` 语法即可，改完执行 `exec fish` 重载。

---

## 五、三个 alias 的使用场景

| alias | 实际命令 | 适用场景 |
|-------|---------|----------|
| `cc` | `claude --dangerously-skip-permissions` | **日常主力**。在任意项目目录下直接启动，跳过权限确认，效率最高 |
| `ccb` | `claude` | **谨慎模式**。对不熟悉的项目，或执行有风险的操作时使用，每步都会询问确认 |
| `cca` | `cd ~/claude/ && claude --dangerously-skip-permissions` | **快速切换**。跳转到个人 claude 工作目录并启动，适合做独立实验或写脚本 |

> **关于 `--dangerously-skip-permissions`**
>
> 这个参数会让 Claude Code 跳过文件读写、命令执行时的权限询问，操作更流畅。
> - ✅ 熟悉 Claude Code 的行为后，推荐默认开启（即用 `cc`）
> - ⚠️ 在涉及生产环境或重要数据的目录操作时，建议改用 `ccb` 保留确认步骤

---

## 六、模型说明

Claude Code 支持多种模型，启动时可通过 `--model` 指定：

| 模型 | 特点 |
|------|------|
| `claude-sonnet-4-6`（默认） | 速度与能力均衡，日常使用首选 |
| `claude-opus-4-8` | 最强推理能力，适合复杂重构、架构设计 |
| `claude-haiku-4-5` | 速度最快，适合简单问答和快速查询 |

```bash
# 指定模型启动
claude --model claude-opus-4-8
```

也可在 `~/.claude/settings.json` 中设置默认模型，避免每次指定：

```json
{
  "model": "claude-sonnet-4-6"
}
```

---

## 七、快速验证

安装并配置 alias 后，在任意目录执行：

```bash
cc
```

看到 Claude Code 的交互界面即为成功 🎉

发送一条测试消息确认正常工作：

```
> 你好，介绍一下你自己
```

---

## 常见问题

**Q：`claude` 命令找不到？**
> 确认 Node.js >= 18（`node -v` 检查），npm 全局 bin 目录是否在 PATH 中（`npm bin -g` 查看路径）。重启终端或执行 `source ~/.zshrc` 后重试。

**Q：alias `cc` 不生效？**
> 执行 `source ~/.zshrc` 后重试；fish 用户执行 `exec fish` 重载配置。

**Q：登录时浏览器没有自动打开？**
> 手动复制终端中显示的 URL 粘贴到浏览器完成授权。

**Q：想用 API Key 而不登录账号？**
> 设置环境变量 `export ANTHROPIC_API_KEY="sk-ant-..."` 后直接运行 `claude`，无需登录。

---

> ← [01 · 终端配置](./01_terminal_setup.md) | **下一篇**：[03 · 唤起与使用 →](./03_launch_and_use.md)
