# 08 · CLAUDE.md 实战 — 把项目规范永久刻进 Claude 的记忆

> **系列索引**：[README](./README.md) · [01](./01_terminal_setup.md) · [02](./02_installation.md) · [03](./03_launch_and_use.md) · [04](./04_shortcuts_and_commands.md) · [05](./05_directory_structure.md) · [06](./06_configuration.md) · [07](./07_ask_user_question.md) · **本篇** · [09](./09_hooks_in_practice.md) · [10](./10_multi_agent_teams.md) · [11](./11_code_review.md) · [12](./12_effective_prompting.md)

---

## 前言

CLAUDE.md 是 Claude Code 最被低估的功能。很多人装好 Claude Code 就直接用，每次都要重复说「用 pnpm」、「不要改这个文件」、「先说方案再动手」。把这些写进 CLAUDE.md，一劳永逸。

本文给出几个真实场景的 CLAUDE.md 模板，以及如何随项目演进持续维护它。

---

## 目录

- [一、CLAUDE.md 的核心价值](#一claudemd-的核心价值)
- [二、写什么 vs 不写什么](#二写什么-vs-不写什么)
- [三、模板：前端项目](#三模板前端项目react--typescript)
- [四、模板：后端项目](#四模板后端项目nodejs--express)
- [五、模板：Monorepo 项目](#五模板monorepo-项目)
- [六、个人全局 CLAUDE.md](#六个人全局-claudemd)
- [七、如何持续维护](#七如何持续维护)

---

## 一、CLAUDE.md 的核心价值

每次启动 Claude Code，它都会加载 CLAUDE.md，相当于给 Claude 的持久化指令。它解决的核心问题是：

- **避免重复说明**：项目规范、禁止操作、常用命令不用每次都说
- **减少低级错误**：明确告诉 Claude Code 哪些文件不能动、哪些命令不能跑
- **统一团队行为**：提交到 git 的 CLAUDE.md 让团队所有人的 Claude Code 行为一致

---

## 二、写什么 vs 不写什么

好的 CLAUDE.md 包含 Claude Code **自己读不到**的信息：

| 类型 | 示例 |
|------|------|
| 项目技术栈 | 用 pnpm、Node 18、TypeScript 严格模式 |
| 常用命令 | 启动、测试、构建、部署命令 |
| 代码规范 | 缩进、命名、注释风格 |
| 禁止操作 | 不能改的文件、不能执行的命令 |
| 工作方式 | 先说方案再动手、遇到不确定先问 |
| 项目特殊说明 | 遗留代码的坑、特殊的业务逻辑 |

**不需要写**的内容：

- 代码本身（Claude Code 会自己读）
- git 历史（Claude Code 会自己查）
- 通用编程知识（Claude Code 已经知道）

---

## 三、模板：前端项目（React + TypeScript）

```markdown
# 项目说明

## 技术栈
- React 18 + TypeScript（严格模式）
- 状态管理：Zustand
- 样式：Tailwind CSS + CSS Modules
- 包管理：pnpm（禁止使用 npm 或 yarn）
- 构建：Vite

## 常用命令
- 启动开发服务器：`pnpm dev`
- 运行测试：`pnpm test`
- 类型检查：`pnpm typecheck`
- 代码检查：`pnpm lint`
- 构建：`pnpm build`

## 代码规范
- 组件文件用 PascalCase（UserCard.tsx）
- 工具函数用 camelCase（formatDate.ts）
- 所有组件必须有 TypeScript Props 类型定义
- 禁止使用 any 类型，用 unknown 替代
- CSS 类名用 Tailwind，复杂样式用 CSS Modules

## 目录结构
- src/components/：通用组件
- src/pages/：页面组件
- src/hooks/：自定义 Hook
- src/stores/：Zustand store
- src/utils/：工具函数

## 禁止操作
- 不要修改 vite.config.ts（构建配置已调优）
- 不要修改 tsconfig.json
- 不要引入新的 UI 组件库（已有 shadcn/ui）
- 不要修改 .env 文件，使用 .env.example 作为参考

## 工作方式
- 修改代码前先说明方案，等我确认后再动手
- 遇到不确定的地方先问我，不要自己假设
- 每次只做一件事，完成后告诉我结果
```

---

## 四、模板：后端项目（Node.js + Express）

```markdown
# 项目说明

## 技术栈
- Node.js 20 + TypeScript
- 框架：Express 4
- 数据库：PostgreSQL + Prisma ORM
- 缓存：Redis
- 包管理：pnpm

## 常用命令
- 启动开发：`pnpm dev`（nodemon 热重载）
- 运行测试：`pnpm test`
- 数据库迁移：`pnpm prisma migrate dev`
- 生成 Prisma Client：`pnpm prisma generate`
- 查看数据库：`pnpm prisma studio`

## 项目结构
- src/routes/：路由定义
- src/controllers/：请求处理
- src/services/：业务逻辑
- src/models/：数据模型（Prisma）
- src/middleware/：中间件
- src/utils/：工具函数

## 数据库规范
- 所有数据库操作通过 Prisma，不写原生 SQL
- 新增字段必须写迁移文件，不要直接改 schema 后 db push
- 禁止在生产环境执行 migrate reset

## API 规范
- 响应格式：{ code: 0, data: {}, message: '' }
- 错误码定义在 src/constants/errors.ts
- 所有接口必须有参数校验（用 zod）
- 认证通过 JWT，token 在 Authorization header

## 禁止操作
- 不要修改 prisma/schema.prisma 中已有的字段类型
- 不要在代码里硬编码密码、token、密钥
- 不要直接操作 Redis，通过 src/services/cache.ts
- 不要修改 src/middleware/auth.ts（认证逻辑已审计）

## 敏感文件（不要读取或修改）
- .env
- .env.production
- src/config/secrets.ts
```

---

## 五、模板：Monorepo 项目

Monorepo 可以在每个子包目录放独立的 CLAUDE.md，Claude Code 进入该目录时自动加载。

**根目录 CLAUDE.md**（全局规范）：

```markdown
# Monorepo 项目说明

## 结构
- packages/web：前端应用（React）
- packages/api：后端服务（Node.js）
- packages/shared：共享类型和工具

## 包管理
- 使用 pnpm workspace
- 安装依赖：`pnpm add <package> --filter <workspace>`
- 运行所有测试：`pnpm test --recursive`

## 跨包修改规范
- 修改 shared 包后必须更新所有依赖它的包
- 不要在 web 或 api 中直接复制 shared 的代码
- 版本号统一在根目录 package.json 管理
```

**packages/api/CLAUDE.md**（子包专属规范）：

```markdown
# API 服务说明

## 特殊说明
- 这个服务部署在 K8s，配置通过环境变量注入
- 端口固定为 3000，不要修改
- 日志格式必须是 JSON

## 本地开发
- 需要先启动 Docker Compose：`docker compose up -d`
- 数据库地址：localhost:5432
```

---

## 六、个人全局 CLAUDE.md

放在 `~/.claude/CLAUDE.md`，对所有项目生效：

```markdown
# 我的工作偏好

## 回复方式
- 始终用中文回复
- 代码修改前先说明方案，等我确认再动手
- 遇到有多种实现方式时，先问我选哪种
- 不要过度解释，直接给结论和关键信息

## 代码习惯
- 不写无意义的注释（代码要自解释）
- 不引入不必要的抽象
- 修改最小化，不顺手「改善」无关代码
- 错误处理只在真正需要的边界加

## 安全习惯
- 不提交 .env 文件
- 不在代码里硬编码任何密钥或密码
- 删除文件前先确认

## 我不喜欢的做法
- 不要在每次回复末尾总结「我做了什么」
- 不要说「当然！」、「好的！」等多余的开场白
- 不要建议我「考虑添加单元测试」（我知道，我自己决定）
```

---

## 七、如何持续维护

CLAUDE.md 不是一次性写好就完事的，要随着项目演进不断更新。

### 触发更新的时机

| 时机 | 操作 |
|------|------|
| Claude Code 做了不该做的事 | 把禁止项加到「禁止操作」 |
| 同一件事说了第二次 | 写进 CLAUDE.md，不再重复 |
| 技术栈变化（换包管理器等） | 更新对应部分 |
| 新团队成员不了解的规则 | 补充说明 |

### 用 /init 快速生成初稿

```
/init
```

Claude Code 会扫描项目自动生成 CLAUDE.md 初稿，再根据实际情况补充修改。

### 定期审查

每隔一段时间，问一下 Claude Code：

```
读一下我们的 CLAUDE.md，有没有哪些规则已经过时了，
或者有哪些常见问题应该加进去？
```

---

> ← [07 · AskUserQuestion](./07_ask_user_question.md) | **下一篇**：[09 · Hooks 实战 →](./09_hooks_in_practice.md)
