# 🤖 如何更高效的驾驭 Claude Code（经验分享）

[![小红书](https://img.shields.io/badge/小红书-关注作者-FF2442?style=flat-square&logo=xiaohongshu&logoColor=white)](https://www.xiaohongshu.com/user/profile/66eaf504000000001d030537)

> 一份面向中文开发者的 Claude Code 实战系列，从终端配置到多 Agent 并行，覆盖入门到进阶的完整路径。

---

## 📚 文章目录

### 入门篇

| # | 标题 | 内容简介 |
|---|------|----------|
| 01 | [打造你的 AI 编码终端](./01_terminal_setup.md) | Ghostty + Starship + Catppuccin 终端环境配置 |
| 02 | [5 分钟装好 Claude Code 并跑起来](./02_installation.md) | 安装、登录认证、alias 配置、模型选择 |
| 03 | [四种方式唤醒 Claude Code](./03_launch_and_use.md) | 终端 / VS Code / Ghostty 联动 / Quick Terminal |
| 04 | [Claude Code 速查手册](./04_shortcuts_and_commands.md) | 快捷键、斜杠命令、CLI flags、Skills 完整速查 |
| 05 | [Claude Code 的文件地图](./05_directory_structure.md) | 全局与项目目录、配置优先级、CLAUDE.md 加载机制 |

### 配置篇

| # | 标题 | 内容简介 |
|---|------|----------|
| 06 | [把 Claude Code 调教成你想要的样子](./06_configuration.md) | settings.json、权限规则、Hooks、提效配置全解 |
| 07 | [别让 Claude 猜](./07_ask_user_question.md) | AskUserQuestion 的触发方式与最佳实践 |
| 08 | [CLAUDE.md 实战](./08_claude_md_in_practice.md) | 前端 / 后端 / Monorepo 模板，持续维护方法 |
| 09 | [Hooks 让 Claude Code 更自动化](./09_hooks_in_practice.md) | 通知、自动 lint、危险命令拦截、调试方法 |

### 进阶篇

| # | 标题 | 内容简介 |
|---|------|----------|
| 10 | [多 Agent 并行工作流](./10_multi_agent_teams.md) | Teams 模式开启、场景示例、已知限制与最佳实践 |
| 11 | [让 Claude Code 做你的代码审查员](./11_code_review.md) | /review 命令、定向审查、集成 git workflow |
| 12 | [高效提问指南](./12_effective_prompting.md) | 上下文给法、任务拆解、低效模式改进、提问模板 |

---

## 🚀 推荐阅读路径

**第一次使用 Claude Code：**
```
01 配好终端 → 02 装好 CC → 03 学会唤起 → 04 掌握命令 → 05 理解目录
```

**想让 Claude Code 更听话：**
```
06 配置调教 → 07 学会提问 → 08 写好 CLAUDE.md → 09 配置 Hooks
```

**想解锁高级用法：**
```
10 多 Agent 并行 → 11 代码审查 → 12 高效提问
```

---

## 💡 核心理念

Claude Code 是运行在终端里的交互式 AI Agent，它的「感知范围」就是你启动时所在的目录：

- **在哪个目录启动，就等于告诉它在哪个项目里工作**
- 它会自动读取文件结构、代码、Git 历史，无需手动粘贴代码
- **CLAUDE.md** 是它的「项目说明书」，写好后每次启动都会加载
- **settings.json** 控制权限和行为，**Hooks** 实现自动化触发

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
