# 🤖 如何更高效的驾驭 Claude Code（经验分享）

作者：[Tassels](https://github.com/Tasselszcx)，美团 Longcat Agent Intern。 [![小红书](https://img.shields.io/badge/小红书-关注作者-FF2442?style=flat-square&logo=xiaohongshu&logoColor=white)](https://www.xiaohongshu.com/user/profile/66eaf504000000001d030537)

> Claude Code 是 Anthropic 官方出品的命令行 AI 编程助手。这份指南不讲理论，只讲**在实际使用中真正有用的东西**。

---

## 怎么用这份指南

**如果你是第一次听说 Claude Code：**
从 Stage 1 开始，按顺序做完，大约 30 分钟后你就能跑起来。

**如果你已经装好但感觉效率一般：**
跳到 Stage 3，重点看配置和 CLAUDE.md，是最快的提效路径。

**如果你想把 Claude Code 用得很深：**
直接看 Stage 4，多 Agent 并行和高效提问是多数人忽略的进阶点。

**如果只找某个具体内容：**
直接看下面的「文章目录」，按需跳转。

---

## 打卡清单

### Stage 1：跑起来

- [ ] 配好终端环境（Ghostty + Starship + Catppuccin）→ [01 终端配置](./01_terminal_setup.md)
- [ ] 安装 Claude Code，配好 `cc` / `ccb` / `cca` 三个 alias → [02 安装](./02_installation.md)
- [ ] 成功启动，向 Claude Code 发出第一条消息 → [02 安装](./02_installation.md)

完成 Stage 1 的标志：在项目目录执行 `cc`，能正常对话。

---

### Stage 2：掌握基础操作

- [ ] 理解「在哪个目录启动 = 告诉它在哪个项目工作」这个核心理念 → [03 唤起与使用](./03_launch_and_use.md)
- [ ] 学会 4 种唤起方式，找到适合自己的工作流 → [03 唤起与使用](./03_launch_and_use.md)
- [ ] 掌握 `@` 引用文件、`#` 添加记忆、`!` 执行命令 三个核心手势 → [04 命令速查](./04_shortcuts_and_commands.md)
- [ ] 理解权限模式，知道什么时候用 `plan` / `bypassPermissions` → [04 命令速查](./04_shortcuts_and_commands.md)
- [ ] 读懂目录结构，知道配置和会话历史放在哪 → [04 命令速查](./04_shortcuts_and_commands.md) · [05 目录结构](./05_directory_structure.md)

完成 Stage 2 的标志：能用 Claude Code 完成一个真实的小任务，不需要手动粘贴代码。

---

### Stage 3：提效配置（这一步最容易拉开差距）

- [ ] 为当前项目写好第一个 `CLAUDE.md`，消灭重复说明 → [08 CLAUDE.md 实战](./08_claude_md_in_practice.md)
- [ ] 配好 `settings.json`，减少权限弹窗，锁定回复语言 → [06 配置全解](./06_configuration.md)
- [ ] 配置至少一个 Hook：任务完成自动桌面通知 → [09 Hooks 实战](./09_hooks_in_practice.md)
- [ ] 学会用 AskUserQuestion 让 Claude 先问再做，避免方向跑偏 → [07 AskUserQuestion](./07_ask_user_question.md)

完成 Stage 3 的标志：启动 Claude Code 不需要重复交代背景，配置一次、处处生效。

---

### Stage 4：进阶用法

- [ ] 用 `/review` 命令做代码审查，写出定向审查的 prompt → [11 代码审查](./11_code_review.md)
- [ ] 尝试 Teams 模式，用多 Agent 并行处理一个复杂任务 → [10 多 Agent 并行](./10_multi_agent_teams.md)
- [ ] 掌握高效提问的核心：给足上下文、拆分任务、明确期望结果 → [12 高效提问](./12_effective_prompting.md)

完成 Stage 4 的标志：能用 Claude Code 完成一个需要多文件协作的功能，并且第一次就做对方向。

---

## 文章目录

### 入门篇

| # | 文章 | 一句话介绍 |
|---|------|----------|
| 01 | [打造你的 AI 编码终端](./01_terminal_setup.md) | Ghostty + Starship + Catppuccin，10 分钟配好开发环境 |
| 02 | [5 分钟装好 Claude Code 并跑起来](./02_installation.md) | 安装、登录、alias 配置、模型选择 |
| 03 | [四种方式唤醒 Claude Code](./03_launch_and_use.md) | 终端 / VS Code / Ghostty 联动 / Quick Terminal |
| 04 | [Claude Code 速查手册](./04_shortcuts_and_commands.md) | 快捷键、斜杠命令、CLI flags、Skills 全收录 |
| 05 | [Claude Code 的文件地图](./05_directory_structure.md) | 全局与项目目录、配置优先级、CLAUDE.md 加载机制 |

### 配置篇

| # | 文章 | 一句话介绍 |
|---|------|----------|
| 06 | [把 Claude Code 调教成你想要的样子](./06_configuration.md) | settings.json、权限规则、Hooks 概览、提效配置 |
| 07 | [别让 Claude 猜](./07_ask_user_question.md) | AskUserQuestion 的触发方式与最佳实践 |
| 08 | [CLAUDE.md 实战](./08_claude_md_in_practice.md) | 前端 / 后端 / Monorepo 模板，持续维护方法 |
| 09 | [Hooks 让 Claude Code 更自动化](./09_hooks_in_practice.md) | 完成通知、自动 lint、危险命令拦截、调试方法 |

### 进阶篇

| # | 文章 | 一句话介绍 |
|---|------|----------|
| 10 | [多 Agent 并行工作流](./10_multi_agent_teams.md) | Teams 模式开启、场景示例、已知限制与最佳实践 |
| 11 | [让 Claude Code 做你的代码审查员](./11_code_review.md) | /review 命令、定向审查、集成 git workflow |
| 12 | [高效提问指南](./12_effective_prompting.md) | 上下文给法、任务拆解、低效模式改进、提问模板 |

---

## 几个最值得记住的点

```
在哪个目录启动 Claude Code，它就在哪个项目里工作。
```

```
CLAUDE.md 是它的「项目说明书」，写一次，永久生效。
```

```
先用 plan 模式对齐方向，再切执行模式动手，返工率大幅降低。
```

```
@ 引用文件 > 手动粘贴代码。让 Claude Code 自己去读，别帮它做。
```

---

## 🛠️ 推荐工具组合

| 工具 | 用途 |
|------|------|
| [Ghostty](https://ghostty.org) | 终端模拟器，原生渲染，Quick Terminal 是杀手功能 |
| [Starship](https://starship.rs) | 跨 Shell 提示符，显示 Git 分支 / 语言版本 / 时间 |
| Maple Mono NF CN | 中英文等宽程序员字体，Nerd Font 图标支持完善 |
| VS Code + Claude Code 插件 | 感知编辑器状态，内联 diff 预览 |

---

如果这份指南对你有帮助，欢迎点个 ⭐ Star，让更多人看到。
