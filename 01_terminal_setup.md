# 01 · 工欲善其事，必先利其器 — 终端环境配置

> **系列索引**：[README](./README.md) · **本篇** · [02 安装 CC](./02_installation.md) · [03 唤起与使用](./03_launch_and_use.md) · [04 命令速查](./04_shortcuts_and_commands.md) · [05 目录结构](./05_directory_structure.md)

---

## 前言

工欲善其事，必先利其器。在正式使用 Claude Code 之前，先花 10 分钟把终端环境配置好——一个好看、好用的终端能让你每天的编码体验提升不止一个档次。

本文推荐的配置组合：

| 工具 | 说明 |
|------|------|
| **Ghostty** | 新一代原生 macOS 终端，性能强、渲染美、专为 AI 编码场景优化 |
| **Maple Mono NF CN** | 专为程序员设计的中英文等宽字体，连字符支持完善 |
| **Starship** | 极速、高度可定制的跨 Shell 提示符 |
| **Catppuccin Mocha** | 低对比度暖色调主题，护眼且美观 |

配置完成后，提示符会显示：**用户名 · 当前目录 · Git 分支状态 · 语言版本 · 时间**，一目了然。

---

## 目录

- [一键安装（推荐）](#一、一键安装推荐)
- [手动配置](#二、手动配置说明)
  - [安装 Ghostty](#1-安装-ghostty)
  - [安装字体](#2-安装字体)
  - [安装 Starship](#3-安装-starship)
  - [Ghostty 配置文件](#4-ghostty-配置文件)
  - [Starship 配置文件](#5-starship-配置文件)
- [Ghostty 常用快捷键](#三、ghostty-常用快捷键)
- [配置文件位置速查](#四、配置文件位置速查)
- [其他终端推荐](#五、其他终端推荐)

---

## 一、一键安装（推荐）

将以下命令粘贴到终端执行，脚本会自动完成所有安装和配置：

```bash
curl -fsSL https://msstest.sankuai.com/videotestbucket/setup-ghostty-new.sh | bash
```

或者下载后执行：

```bash
# 1. 下载脚本
curl -O https://msstest.sankuai.com/videotestbucket/setup-ghostty-new.sh

# 2. 添加执行权限
chmod +x setup-ghostty-new.sh

# 3. 执行安装
./setup-ghostty-new.sh
```

脚本会依次完成：

1. 检查并安装 Homebrew（如未安装）
2. 检查并安装 Ghostty
3. 安装 Maple Mono NF CN 字体
4. 安装 Starship
5. 写入 Ghostty 配置（Catppuccin Mocha 主题 + 分屏快捷键）
6. 写入 Starship 配置（Catppuccin Mocha 风格提示符）
7. 自动检测 Shell 并配置 Starship 初始化

> ✅ **安全提示**：已有配置会自动备份为 `*.bak.<时间戳>`，不会覆盖丢失。

---

## 二、手动配置说明

如果你希望逐步了解每一步，或者需要定制某些选项，可以按下面的步骤手动操作。

### 1. 安装 Ghostty

```bash
brew install --cask ghostty
```

安装完成后在 Launchpad 或 Spotlight 中搜索 **Ghostty** 打开。

### 2. 安装字体

```bash
brew install --cask font-maple-mono-nf-cn
```

安装后需要在 Ghostty 配置文件中指定该字体（见第 4 步）。

### 3. 安装 Starship

```bash
brew install starship
```

根据你使用的 Shell，在配置文件末尾添加初始化命令：

| Shell | 配置文件 | 初始化命令 |
|-------|---------|------------|
| fish  | `~/.config/fish/config.fish` | `starship init fish \| source` |
| zsh   | `~/.zshrc` | `eval "$(starship init zsh)"` |
| bash  | `~/.bashrc` | `eval "$(starship init bash)"` |

执行 `exec $SHELL` 或重启终端使配置生效。

### 4. Ghostty 配置文件

配置文件路径：`~/.config/ghostty/config`（不存在则新建）

核心外观配置说明：

```ini
# 字体与字号
font-family = "Maple Mono NF CN"
font-size = 14

# 主题
theme = Catppuccin Mocha

# 背景毛玻璃效果（0~1，越小越透明）
background-opacity = 0.9
background-blur-radius = 30

# 标题栏
macos-titlebar-style = transparent

# 光标
cursor-style = bar
cursor-style-blink = true

# 内边距
window-padding-x = 10
window-padding-y = 8

# 分屏失焦透明度
unfocused-split-opacity = 0.7
```

> 💡 修改配置后按 `Cmd+Shift+,` 重载，无需重启 Ghostty。

### 5. Starship 配置文件

配置文件路径：`~/.config/starship.toml`

> ⚠️ **注意**：`format` 字段中的过渡字符必须是正确的 Nerd Font 字符。直接复制纯文本时字符可能丢失，导致提示符显示为独立色块而非连续胶囊条。建议通过脚本安装或直接复制下方配置文件。

完整 `starship.toml` 配置（Catppuccin Mocha 风格连续胶囊提示符）：

```toml
"$schema" = 'https://starship.rs/config-schema.json'

format = """
[](red)\
$os\
$username\
[](bg:peach fg:red)\
$directory\
[](bg:yellow fg:peach)\
$git_branch\
$git_status\
[](fg:yellow bg:green)\
$c\
$rust\
$golang\
$nodejs\
$php\
$java\
$kotlin\
$haskell\
$python\
[](fg:green bg:sapphire)\
$conda\
[](fg:sapphire bg:lavender)\
$time\
[](fg:lavender)\
$cmd_duration\
$line_break\
$character"""

palette = 'catppuccin_mocha'

[os]
disabled = false
style = "bg:red fg:crust"

[os.symbols]
Macos = "󰀵"

[username]
show_always = true
style_user = "bg:red fg:crust"
style_root = "bg:red fg:crust"
format = '[ $user]($style)'

[directory]
style = "bg:peach fg:crust"
format = "[ $path ]($style)"
truncation_length = 3
truncation_symbol = "…/"

[git_branch]
symbol = ""
style = "bg:yellow"
format = '[[ $symbol $branch ](fg:crust bg:yellow)]($style)'

[git_status]
style = "bg:yellow"
format = '[[($all_status$ahead_behind )](fg:crust bg:yellow)]($style)'

[nodejs]
symbol = ""
style = "bg:green"
format = '[[ $symbol( $version) ](fg:crust bg:green)]($style)'

[golang]
symbol = ""
style = "bg:green"
format = '[[ $symbol( $version) ](fg:crust bg:green)]($style)'

[python]
symbol = ""
style = "bg:green"
format = '[[ $symbol( $version)(\(#$virtualenv\)) ](fg:crust bg:green)]($style)'

[time]
disabled = false
time_format = "%R"
style = "bg:lavender"
format = '[[  $time ](fg:crust bg:lavender)]($style)'

[character]
disabled = false
success_symbol = '[❯](bold fg:green)'
error_symbol = '[❯](bold fg:red)'

[cmd_duration]
show_milliseconds = true
format = " in $duration "
disabled = false

[palettes.catppuccin_mocha]
rosewater = "#f5e0dc"
flamingo  = "#f2cdcd"
pink      = "#f5c2e7"
mauve     = "#cba6f7"
red       = "#f38ba8"
maroon    = "#eba0ac"
peach     = "#fab387"
yellow    = "#f9e2af"
green     = "#a6e3a1"
teal      = "#94e2d5"
sky       = "#89dceb"
sapphire  = "#74c7ec"
blue      = "#89b4fa"
lavender  = "#b4befe"
text      = "#cdd6f4"
subtext1  = "#bac2de"
subtext0  = "#a6adc8"
overlay2  = "#9399b2"
overlay1  = "#7f849c"
overlay0  = "#6c7086"
surface2  = "#585b70"
surface1  = "#45475a"
surface0  = "#313244"
base      = "#1e1e2e"
mantle    = "#181825"
crust     = "#11111b"
```

---

## 三、Ghostty 常用快捷键

| 快捷键 | 功能 |
|--------|------|
| `Cmd+Space` | 呼出 / 隐藏 Quick Terminal（全局，任意应用均可触发） |
| `Cmd+D` | 向右分屏 |
| `Cmd+Shift+Enter` | 当前分屏全屏 / 还原 |
| `Cmd+Shift+,` | 重载配置文件（修改配置后无需重启） |
| `Cmd+[` / `Cmd+]` | 切换分屏 |
| `Cmd+T` | 新建标签页 |
| `Cmd+W` | 关闭当前窗口 / 分屏 |
| `Cmd++` / `Cmd+-` | 字体放大 / 缩小 |

> 🌟 **Quick Terminal** 是 Ghostty 的杀手级功能：在任何应用中按 `Cmd+Space` 即可从屏幕顶部滑出终端，再按一次收起，完全不打断当前工作流。

---

## 四、配置文件位置速查

| 工具 | 配置文件路径 |
|------|-------------|
| Ghostty | `~/.config/ghostty/config` |
| Starship | `~/.config/starship.toml` |
| fish | `~/.config/fish/config.fish` |
| zsh | `~/.zshrc` |
| bash | `~/.bashrc` |

---

## 五、其他终端推荐

除 Ghostty 外，以下终端也能很好地配合 Claude Code 使用：

### cmux — 专为 AI 编码代理打造

[cmux](https://github.com/manaflow-ai/cmux) 基于 Ghostty 渲染引擎，专门针对 Claude Code 等 AI 编码代理场景设计。

**核心亮点：**
- 🔵 **智能通知**：Claude Code 需要人工介入时，窗格边框亮起蓝色圆环
- 👥 **原生 Teams 模式**：内置多 Agent 并行支持，无需配置 tmux
- 🌐 **内置浏览器分屏**：终端旁直接分割显示浏览器窗口
- ⚡ **Swift 原生**：基于 libghostty 渲染，非 Electron，资源占用低

> 注：实测可能有卡顿，根据个人情况选择。

### iTerm2 — 最成熟的 macOS 终端

[iTerm2](https://iterm2.com) 是 Mac 上使用最广泛的第三方终端，生态成熟，tmux 集成完善。如果你已经习惯 iTerm2，配合 tmux 使用 Claude Code 完全没问题，无需切换。

### Warp — AI 原生终端

[Warp](https://www.warp.dev) 内置 AI 命令建议和自然语言搜索，界面现代，对新手友好。本身就是 AI 终端，与 Claude Code 场景有一定重叠，按需选择。

### otty

[https://otty.sh](https://otty.sh/?ref=appmakes) — 轻量级新兴选手，可关注。

---

> **下一篇**：[02 · 安装 Claude Code →](./02_installation.md)
