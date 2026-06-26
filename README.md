# 🤖 How to Use Claude Code Better

> 一份面向中文开发者的 Claude Code 实战入门系列，从环境配置到高级用法，快速上手 AI 驱动的编码工作流。

---

## 📚 文章目录

| # | 标题 | 内容简介 |
|---|------|----------|
| 01 | [工欲善其事，必先利其器](./01_terminal_setup.md) | Ghostty + Starship + Catppuccin 终端环境一键配置 |
| 02 | [安装 Claude Code](./02_installation.md) | CatPaw CLI 安装、启动方式与常用 alias |
| 03 | [唤起并使用 Claude Code](./03_launch_and_use.md) | 终端、VS Code、Ghostty 三种主力工作流 |
| 04 | [主要快捷键、命令与使用方法](./04_shortcuts_and_commands.md) | 快捷键、斜杠命令、CLI flags、Skills 完整速查 |
| 05 | [CC 的主要目录结构](./05_directory_structure.md) | 全局与项目目录、配置优先级、CLAUDE.md 加载机制 |

---

## 🚀 快速开始

如果你是第一次使用 Claude Code，建议按顺序阅读：

```
01 配好终端 → 02 装好 CC → 03 学会唤起 → 04 掌握命令 → 05 理解目录
```

如果你已经安装好了，可以直接跳到 [04 快捷键速查](./04_shortcuts_and_commands.md)。

---

## 💡 核心理念

Claude Code 是一个运行在终端里的交互式 AI Agent。它的「感知范围」就是你启动时所在的目录——所以：

- **在哪个目录启动，就等于告诉它在哪个项目里工作**
- 它会自动读取文件结构、代码、Git 历史，你无需手动粘贴代码
- CLAUDE.md 是它的「项目说明书」，写好后每次启动都会加载

---

## 🛠️ 推荐工具组合

| 工具 | 用途 | 推荐指数 |
|------|------|---------|
| [Ghostty](https://ghostty.org) | 终端模拟器（原生、快速、美观） | ⭐⭐⭐⭐⭐ |
| [Starship](https://starship.rs) | 跨 Shell 提示符 | ⭐⭐⭐⭐⭐ |
| Maple Mono NF CN | 中英文程序员字体 | ⭐⭐⭐⭐⭐ |
| VS Code + Claude Code 插件 | 编辑器联动 | ⭐⭐⭐⭐ |

---

## 📝 贡献与反馈

欢迎提 Issue 或 PR，一起把这份指南做得更好。

