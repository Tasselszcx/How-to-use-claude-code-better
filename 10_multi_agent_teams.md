# 10 · 多 Agent 并行 — 用 Teams 模式解锁真正的并行工作流

> **系列索引**：[README](./README.md) · [01](./01_terminal_setup.md) · [02](./02_installation.md) · [03](./03_launch_and_use.md) · [04](./04_shortcuts_and_commands.md) · [05](./05_directory_structure.md) · [06](./06_configuration.md) · [07](./07_ask_user_question.md) · [08](./08_claude_md_in_practice.md) · [09](./09_hooks_in_practice.md) · **本篇** · [11](./11_code_review.md) · [12](./12_effective_prompting.md)

---

## 前言

大多数时候，单个 Claude Code 会话就够用了。但有些任务天然适合并行：同时审查代码的安全性、性能和测试覆盖；同时开发前端、后端和测试；同时用多个假设调查一个 bug……

Teams 模式让多个 Claude Code 实例协同工作，每个 Agent 有独立的上下文窗口，通过共享任务列表和消息系统协调。

> **注意**：Teams 模式目前是实验性功能，需要手动开启，且有一些已知限制。

---

## 目录

- [一、Teams 模式 vs 普通模式](#一teams-模式-vs-普通模式)
- [二、开启 Teams 模式](#二开启-teams-模式)
- [三、启动第一个 Agent Team](#三启动第一个-agent-team)
- [四、显示模式](#四显示模式)
- [五、实用场景示例](#五实用场景示例)
- [六、控制 Agent 团队](#六控制-agent-团队)
- [七、已知限制](#七已知限制)
- [八、最佳实践](#八最佳实践)

---

## 一、Teams 模式 vs 普通模式

| 对比项 | 单 Agent | Teams 模式 |
|--------|---------|------------|
| 上下文 | 单个窗口 | 每个 Agent 独立窗口 |
| 并行能力 | 顺序执行 | 真正并行 |
| Agent 间通信 | 无 | 可以直接互发消息 |
| Token 消耗 | 低 | 高（按 Agent 数量线性增加） |
| 适合场景 | 大多数任务 | 可独立并行的复杂任务 |

### 什么时候用 Teams 模式

- ✅ 多个子任务之间没有依赖，可以同时进行
- ✅ 需要多视角分析（安全、性能、可读性各一个 Agent）
- ✅ 大型 feature 跨多个模块，每个模块独立开发
- ✅ 调试时有多个互相竞争的假设，需要同时验证

### 什么时候不用

- ❌ 任务有严格的顺序依赖
- ❌ 多个 Agent 会修改同一个文件（会产生冲突）
- ❌ 简单任务（协调开销大于收益）

---

## 二、开启 Teams 模式

Teams 模式默认关闭，需要在配置中开启：

```json
// ~/.claude/settings.json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

或者在启动时临时开启：

```bash
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

---

## 三、启动第一个 Agent Team

开启后，直接用自然语言告诉 Claude Code 你想要什么样的团队：

```
我需要对 src/auth/ 模块做全面审查。
创建一个 Agent 团队，三个成员分别负责：
- 安全性审查（重点关注认证和授权逻辑）
- 性能审查（重点关注数据库查询和缓存使用）
- 测试覆盖审查（检查是否有遗漏的边界情况）
```

Claude Code 会自动创建团队、分配任务、协调工作，最后汇总结果。

---

## 四、显示模式

Teams 模式有两种显示方式：

### in-process 模式（默认）

所有 Agent 在同一个终端里运行：

| 快捷键 | 功能 |
|--------|------|
| `Shift+↓` | 切换到下一个 Agent |
| `Ctrl+T` | 切换任务列表显示 |

### split-pane 模式（需要 tmux 或 iTerm2）

每个 Agent 在独立的分屏窗格里，可以同时看到所有 Agent 的输出。

```json
// 全局设置 split-pane 模式
{
  "teammateMode": "tmux"
}
```

或者单次启动时指定：

```bash
claude --teammate-mode tmux
```

> 注意：split-pane 模式不支持 VS Code 集成终端和 Ghostty，推荐在 iTerm2 或配合 tmux 使用。

---

## 五、实用场景示例

### 场景一：并行代码审查

```
对 PR #42 进行多维度审查。
创建 3 个审查员 Agent 并行工作：

- security-reviewer：专注安全漏洞（SQL 注入、XSS、认证绕过等）
- perf-reviewer：专注性能问题（N+1 查询、不必要的循环、内存泄漏等）
- test-reviewer：专注测试覆盖（边界情况、错误路径、mock 是否合理）

各自完成后向我汇报发现的问题。
```

### 场景二：并行 feature 开发

```
需要实现用户通知系统，包含三个独立部分。
创建 Agent 团队并行开发：

- backend-agent：实现 src/notifications/ 的后端逻辑和数据库模型
- frontend-agent：实现 src/components/Notifications/ 的 UI 组件
- test-agent：为以上两部分编写测试

注意：backend-agent 完成 API 接口定义后，
frontend-agent 才能开始对接，需要先协调好接口格式。
```

### 场景三：多假设并行调试

```
用户反馈在高并发下偶现数据不一致。
我有三个怀疑方向，创建 Agent 团队同时验证：

- hypothesis-1：检查 src/services/order.ts 里是否有竞态条件
- hypothesis-2：检查数据库事务隔离级别配置是否正确
- hypothesis-3：检查 Redis 缓存更新逻辑是否有问题

每个 Agent 独立调查，互相质疑对方的结论，最后给出最可能的根因。
```

---

## 六、控制 Agent 团队

### 直接和某个 Agent 对话

在 in-process 模式下，`Shift+↓` 切换到目标 Agent，直接输入消息。

### 给 Agent 追加指令

```
告诉 security-reviewer：重点检查 JWT token 的验证逻辑，
特别是 token 过期处理和签名验证
```

### 要求计划审批

对于风险较高的任务，可以要求 Agent 先给出计划再执行：

```
创建一个 refactor-agent 来重构认证模块。
要求它先制定重构计划，等我审批后再开始修改代码。
```

### 清理团队

任务完成后清理团队资源：

```
任务完成了，请清理团队
```

---

## 七、已知限制

使用前需要了解的限制：

| 限制 | 说明 |
|------|------|
| 不支持会话恢复 | `/resume` 无法恢复 in-process 的 teammate，重启后需要重新创建 |
| 任务状态可能滞后 | Agent 有时不会自动标记任务完成，需要手动催促 |
| 不支持嵌套团队 | teammate 不能再创建自己的子团队 |
| 文件冲突风险 | 两个 Agent 修改同一文件会产生覆盖，需要提前规划好分工 |
| Token 消耗高 | 每个 Agent 独立计费，谨慎控制团队规模 |

> **推荐团队规模**：3–5 个 Agent，任务量约 5–6 个/Agent，超过这个规模协调开销会显著增加。

---

## 八、最佳实践

1. **先明确分工边界**：启动前想清楚每个 Agent 负责哪些文件，避免冲突
2. **任务粒度适中**：太小的任务不值得并行，太大的任务容易出错
3. **给每个 Agent 足够的上下文**：在创建指令中说清楚背景，Agent 不会继承主会话的历史记录
4. **及时介入**：不要让团队完全无人监管，定期检查进度并调整方向
5. **从简单场景开始**：先用代码审查这类只读任务熟悉 Teams 模式，再尝试并行开发

---

> ← [09 · Hooks 实战](./09_hooks_in_practice.md) | **下一篇**：[11 · 代码审查 →](./11_code_review.md)
