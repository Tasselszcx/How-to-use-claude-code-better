# 02 · 安装 Claude Code

> **系列索引**：[README](./README.md) · [01 终端配置](./01_terminal_setup.md) · **本篇** · [03 唤起与使用](./03_launch_and_use.md) · [04 命令速查](./04_shortcuts_and_commands.md) · [05 目录结构](./05_directory_structure.md)

---

## 前言

上一篇我们配好了 Ghostty + Starship 的终端环境。这一篇讲 Claude Code 的**安装与启动**。

在美团内网，Claude Code 通过公司提供的 **CatPaw CLI**（`mc` 命令）代理启动，不需要自己配置 API Key，开箱即用。

---

## 目录

- [一、安装 CatPaw CLI（mc）](#一安装-catpaw-climc)
- [二、安装 Claude Code](#二安装-claude-code)
- [三、启动方式](#三启动方式)
- [四、三个 alias 的使用场景](#四三个-alias-的使用场景)
- [五、模型说明](#五模型说明)
- [六、快速验证](#六快速验证)

---

## 一、安装 CatPaw CLI（mc）

CatPaw CLI 是美团内部的 AI 命令行工具，`mc --code` 是它启动 Claude Code 的入口。

```bash
bash -c "$(curl -sL https://s3plus.sankuai.com/mcopilot-cli/install.sh)"
```

安装完成后验证：

```bash
mc --version
```

> **遇到权限错误？** 如果提示 `/usr/local/bin/mc: Permission denied`，在命令前加 `sudo` 重试：
> ```bash
> sudo bash -c "$(curl -sL https://s3plus.sankuai.com/mcopilot-cli/install.sh)"
> ```

---

## 二、安装 Claude Code

CatPaw CLI 安装好后，还需要本地有 Claude Code 的二进制文件。两种方式二选一：

```bash
# 方式一：官方安装脚本（需能访问 claude.ai）
curl -fsSL https://claude.ai/install.sh | bash

# 方式二：npm 安装（要求 Node.js >= 18）
npm install -g @anthropic-ai/claude-code
```

安装完成后验证：

```bash
claude --version
```

---

## 三、启动方式

### 原始命令

```bash
# 普通模式：每次操作需要手动确认权限（适合新手或敏感操作）
mc --code

# Yolo 模式：跳过所有权限确认（适合熟悉后的日常使用）
mc --code --dangerously-skip-permissions
```

### 推荐：配置 alias 一键启动

每次手动输入 `mc --code --dangerously-skip-permissions` 太繁琐，配置 alias 后只需两个字母即可启动。

在 `~/.zshrc`（或 `~/.bashrc`）末尾追加：

```bash
# Claude Code alias 配置
alias cc='mc --code --dangerously-skip-permissions'           # 日常主力：yolo 模式
alias ccb='mc --code'                                          # 谨慎模式：需手动确认
alias cca='cd ~/claude/ && mc --code --dangerously-skip-permissions'  # 在工作目录启动
```

配置后执行以下命令使其立即生效：

```bash
source ~/.zshrc
```

---

## 四、三个 alias 的使用场景

| alias | 实际命令 | 适用场景 |
|-------|---------|----------|
| `cc` | `mc --code --dangerously-skip-permissions` | **日常主力**。在任意项目目录下直接启动，跳过权限确认，效率最高 |
| `ccb` | `mc --code` | **谨慎模式**。对不熟悉的项目，或执行有风险的操作时使用，每步都会询问确认 |
| `cca` | `cd ~/claude/ && mc --code --dangerously-skip-permissions` | **快速切换**。跳转到个人 claude 工作目录并启动，适合做独立实验或写脚本 |

> **关于 `--dangerously-skip-permissions`**
>
> 这个参数会让 Claude Code 跳过文件读写、命令执行时的权限询问，操作更流畅。
> - ✅ 熟悉 Claude Code 的行为后，推荐默认开启（即用 `cc`）
> - ⚠️ 在生产相关目录操作时，建议改用 `ccb` 保留确认步骤

---

## 五、模型说明

通过 `mc --code` 启动时，默认使用 **Claude Sonnet 模型**（当前路由到 Sonnet 4.6）。

| 模型 | 状态 | 说明 |
|------|------|------|
| Claude Sonnet 4.6 | ✅ 可用 | 默认模型，速度快、能力强 |
| Claude Opus | ⚠️ 暂不支持 | 会自动路由到 Sonnet 4.6 |

---

## 六、快速验证

安装完成后，在任意目录执行：

```bash
cc
```

看到 Claude Code 的交互界面即为成功 🎉

可以先发送一条测试消息：

```
> 你好，介绍一下你自己
```

如果 Claude Code 正常回复，说明一切配置正确。

---

## 常见问题

**Q：`mc` 命令找不到？**
> 重启终端，或手动执行 `source ~/.zshrc`。如果还是找不到，检查安装脚本是否报错。

**Q：alias `cc` 不生效？**
> 执行 `source ~/.zshrc` 后重试；若使用 fish，修改 `~/.config/fish/config.fish` 并用 `exec fish` 重载。

**Q：启动后出现认证错误？**
> 确认 CatPaw CLI 已正确安装（`mc --version` 有输出），且在内网环境下操作。

---

> ← [01 · 终端配置](./01_terminal_setup.md) | **下一篇**：[03 · 唤起与使用 →](./03_launch_and_use.md)
