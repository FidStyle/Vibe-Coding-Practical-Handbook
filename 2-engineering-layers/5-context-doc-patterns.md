<div data-theme-toc="true"></div>

# 2.5 上下文文档的分层与维护模式

> 阅读完本节后，你应该能为一个项目设计最小上下文文档结构，让 `Codex`、`Claude Code` 和 `OpenCode` 每次进入项目时都能更快理解边界。

`Context Engineering` 不是把所有资料提供给模型。它要做的是：把正确的上下文放在正确的位置，让 Agent 需要时能找到。

## 上下文不是越多越好

很多人把项目 README、需求文档、聊天记录、报错日志、接口文档一次性全部提供给 Agent。结果通常更差：

- 关键信息被噪音稀释。
- 旧决策和新决策互相冲突。
- Agent 不知道哪份文档权威。
- 上下文窗口被无效内容占满。

更稳的做法是分层。

```text
长期稳定上下文
  项目目标、架构、目录约定、编码规范

中期任务上下文
  当前 PRD、设计方案、任务拆分、验收标准

短期会话上下文
  当前报错、当前 diff、刚刚运行的测试输出
```

## 推荐文档层级

| 层级 | 文件 | 作用 |
|---|---|---|
| 项目入口 | `README.md` | 给人和 Agent 的项目概览 |
| Agent 规则 | `AGENTS.md` / `CLAUDE.md` | 工具专用的工作规则 |
| 架构说明 | `docs/architecture.md` | 模块边界、数据流、关键决策 |
| 命令说明 | `docs/commands.md` | install、dev、test、lint、build |
| 任务说明 | `tasks/<task>/prd.md` | 当前需求、验收、非目标 |
| 实施计划 | `tasks/<task>/plan.md` | 文件范围、步骤、风险 |
| 会话记录 | `tasks/<task>/journal.md` | 已完成、失败点、下一步 |
| 规范沉淀 | `docs/conventions/*.md` | 从多次任务中抽出的稳定规则 |

你不需要一次建全。最小组合是：

```text
README.md
AGENTS.md / CLAUDE.md / opencode.json
docs/commands.md
tasks/current/prd.md
```

## `README.md` 写给谁

`README.md` 应该让新人和 Agent 在 3 分钟内知道：

- 项目做什么。
- 主要技术栈是什么。
- 入口目录在哪里。
- 怎么启动。
- 怎么验证。
- 哪些目录不要随便改。

不要把所有细节写入 README。它是索引，不是百科。

## `AGENTS.md` / `CLAUDE.md` 写什么

这类文件应该简短、明确、可执行。

```markdown
# Agent Instructions

## Project Map
- `src/app/`: 页面和 API 路由。
- `src/lib/`: 可复用业务逻辑。
- `tests/`: 单元测试和集成测试。

## Commands
- Install: `pnpm install`
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`
- Build: `pnpm build`

## Rules
- 不要引入新依赖，除非任务明确要求。
- 不要修改 `.env`。
- 改公共接口必须同步测试和文档。
- 大改前先给计划。

## Done Means
- 相关测试通过。
- diff 中没有无关格式化。
- 如果命令无法运行，说明原因和替代验证方式。
```

## 目录级上下文

当项目变大时，可以在关键目录放局部说明。

```text
src/
  AGENTS.md
  billing/
    README.md
  auth/
    README.md
```

目录级文档只写本目录特有规则：

- 输入是什么。
- 输出是什么。
- 依赖谁。
- 不能破坏什么兼容性。
- 常见风险或易错点是什么。

## 分层文档结构

可以借用社区里的分层文档思路：每一层文档都只解释自己这一层。

```text
根文档
  解释项目全局目标和入口。

模块文档
  解释模块职责、边界、数据流。

文件头注释或局部说明
  解释复杂函数的输入、输出和约束。
```

这样 Agent 不需要一次读完所有文档。它可以从根文档进入，再按任务读到具体模块。

## 维护规则

上下文文档一过期，就会反过来误导 Agent。建议在 `AGENTS.md` 或 `CLAUDE.md` 里加一条硬规则：

```markdown
如果本次改动改变了命令、目录结构、公共接口、数据模型或关键约定，必须同步更新相关文档。
```

每次任务结束让 Agent 自查：

```text
请检查本次 diff 是否导致 README、AGENTS.md、CLAUDE.md、docs/ 或任务文档需要同步更新。
如果需要，列出原因并做最小文档更新。
```

## 文档权威性

一个项目里可能有很多文档。需要明确优先级。

```text
用户当前明确指令
  > 当前任务 PRD
  > AGENTS.md / CLAUDE.md
  > docs/architecture.md
  > README.md
  > 历史会话记录
```

如果文档冲突，Agent 应该停下来问，或者提出它准备采用哪一个。

## 常见坏写法

| 坏写法 | 后果 | 修正 |
|---|---|---|
| 把聊天记录当文档 | 决策和废案混在一起 | 提炼成 PRD / ADR |
| README 巨大无比 | Agent 找不到重点 | README 做索引，细节下沉 |
| 规则写得很虚 | Agent 无法执行 | 改成命令、路径、禁止项 |
| 文档没人更新 | 上下文污染 | 把文档同步写进任务完成标准 |
| 每个工具一套冲突规则 | 行为不一致 | 抽一份共用规则，再做工具适配 |

## 最小实践

如果只做最小改进，先完成三件事：

1. 在项目根目录写一个 60 行以内的 `AGENTS.md` 或 `CLAUDE.md`。
2. 新建 `docs/commands.md`，写清楚所有验证命令。
3. 每个任务新建一个 `tasks/current/prd.md`，让 Agent 先读它再动手。
