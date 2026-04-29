<div data-theme-toc="true"></div>

# 7.5 从上手到独立开发的 30 天路线

> 读完本节后，你可以按 30 天节奏练习，从网页对话过渡到用 `Claude Code` / `Codex` / `OpenCode` 独立完成真实项目。

这条路线面向刚接触 Vibe Coding 的读者。会编程、会 Git、会终端会更快；暂时不会，就把前两周当成发现缺口的过程，哪一步过不去，就回到环境准备、项目 README 和 Git 基础补到能继续练。

## 总目标

30 天结束时，你要能独立完成这些小项目：

```text
一个小型 Web 项目
一个自动化脚本
一个个人工作流工具
一次 bugfix / refactor
一套最小 harness
```

重点在完整循环，不在项目数量：

```mermaid
flowchart LR
  A[需求] --> B[交接卡]
  B --> C[本地 Agent]
  C --> D[验证]
  D --> E[审查]
  E --> F[文档沉淀]
```

## 第 1 周：从网页对话到任务定义

目标：不要再把网页对话当开发环境。它适合澄清需求、探索方案，不适合直接接管仓库。

| 天 | 任务 | 产出 |
|---|---|---|
| Day 1 | 选一个你想做的小工具 | 一页想法说明 |
| Day 2 | 用网页 GPT / Claude 拆需求 | PRD 初稿 |
| Day 3 | 用 Gemini 或长上下文工具整理资料 | 背景摘要 |
| Day 4 | 把 PRD 改成 Task Handoff | 交接卡 |
| Day 5 | 为任务写完成条件 | 验收标准 |
| Day 6 | 找一个已有本地仓库作为练习对象 | 仓库地图 |
| Day 7 | 复盘网页对话哪些有用、哪些该丢弃 | lessons learned |

这一周合格的标志：

- 每个任务都写清 `Out of Scope`（本次不做什么）。
- 每个验收标准都可验证。
- 不把网页模型生成的代码直接粘进项目。

## 第 2 周：本地 Agent 基础循环

目标：把下面这条闭环稳定执行，不要只让 Agent “尝试一下”。

```mermaid
flowchart LR
  A[Research] --> B[Plan]
  B --> C[Implement]
  C --> D[Validate]
  D --> E[Review<br/>审查]
```

| 天 | 任务 | 产出 |
|---|---|---|
| Day 8 | 让 Claude Code / Codex / OpenCode 只读分析本地仓库 | 文件地图 |
| Day 9 | 做一个小 bugfix | 最小 diff |
| Day 10 | 给已有逻辑补测试 | 测试 diff |
| Day 11 | 做一个小功能 | 功能 diff |
| Day 12 | 故意让 Agent 先计划再执行 | plan.md |
| Day 13 | 跑测试、lint、typecheck | validation.md |
| Day 14 | 让 Agent 审查自己的 diff | 审查记录 |

这一周合格的标志：

- 非平凡任务都先 read-only。
- 你能指出 Agent 改了哪些文件、为什么改。
- 至少处理过一次测试失败。

## 第 3 周：MCP、skills 和最小 harness

目标：把重复上下文搬运和重复流程自动化，减少对临时提醒的依赖。

| 天 | 任务 | 产出 |
|---|---|---|
| Day 15 | 写项目 `AGENTS.md` 或 `CLAUDE.md` | Agent 规则 |
| Day 16 | 写 `docs/commands.md` | 命令索引 |
| Day 17 | 接一个只读文档 MCP | MCP 验证记录 |
| Day 18 | 接一个浏览器或 Git MCP | 工具调用记录 |
| Day 19 | 把 PR 审查提示词做成 skill | SKILL.md |
| Day 20 | 用 skill 跑一次真实审查 | findings |
| Day 21 | 复盘哪些规则该沉淀 | 更新文档 |

这一周合格的标志：

- MCP 至少有一次成功调用和一次失败处理记录。
- skill 有明确触发场景和输出格式。
- `AGENTS.md` / `CLAUDE.md` 控制在可读范围，不变成无序收集文件。

## 第 4 周：独立完成一个综合项目

目标：从需求到交付独立完成一次。规模不必大，但流程要完整。

项目三选一即可：

| 项目 | 内容 |
|---|---|
| Web 小项目 | 资料收藏、任务看板、账单分析、文件索引 |
| 自动化工具 | 批量整理文件、生成 CSV、抓取公开数据、日志分析 |
| 工作流工具 | issue triage、PR 审查、学习资料索引、周报生成 |

| 天 | 任务 | 产出 |
|---|---|---|
| Day 22 | 写 PRD 和任务目录 | `tasks/project/prd.md` |
| Day 23 | 让 Agent 做架构调研和计划 | `plan.md` |
| Day 24 | 实现第一个可运行版本 | MVP diff |
| Day 25 | 补核心测试和错误处理 | tests |
| Day 26 | 接入一个 MCP 或写一个 skill | 扩展能力 |
| Day 27 | 做一次重构或性能改进 | refactor diff |
| Day 28 | 完整验证和文档同步 | validation |
| Day 29 | 让 Agent 做代码审查 | 审查报告 |
| Day 30 | 复盘并整理个人实用做法 | personal playbook |

## 30 天后的能力标准

到这里，你要能做到：

- 用网页模型澄清需求，但不依赖网页模型实现代码。
- 用 `Claude Code` / `Codex` / `OpenCode` 在真实本地仓库里小步交付。
- 能判断何时用 MCP，何时写 skill。
- 能为项目搭最小 harness。
- 能处理 Agent 偏离目标和测试失败。
- 能通过审查和验证控制质量。

## 失败也算训练

如果 30 天里某天失败，不要补做更多代码。写复盘：

```text
失败发生在哪一层？
- Prompt 不清楚？
- Context 不足？
- Harness 缺失？
- 工具选错？
- 验证太晚？
```

需要训练的是定位能力：失败本身可接受，关键是定位失败所在层。复盘要落到下一次任务能用的规则、模板或检查项。
