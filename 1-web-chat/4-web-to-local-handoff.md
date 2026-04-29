<div data-theme-toc="true"></div>

# 1.4 网页对话到本地 Agent 的交接卡

> 阅读完本节后，你应该能把一次 GPT / Claude / Gemini 网页对话，压缩成 `Codex`、`Claude Code` 或 `OpenCode` 能先分析、再执行的任务说明。

网页对话有用，但不适合负责真实项目执行：网页模型无法稳定读取你的本地仓库、运行命令、看测试结果、维护 diff。更稳的做法是让它先帮你把问题想清楚，再把结果交给本地 Agent 执行。

## 典型场景

你在网页 Claude 里讨论了一个需求：

```text
我想给现有 Next.js 项目加一个批量导入 CSV 的功能。
用户上传 CSV，系统校验字段，展示错误行，确认后写入数据库。
```

网页模型可以帮你想清楚用户流程、数据字段、错误处理、API 形状、测试点和风险。但它无法读取你的本地仓库，因此不要让它推断项目结构。到执行阶段，就切到本地 Agent：先读代码，再给计划。

## 交接卡结构

一张合格的交接卡包含 8 个部分。

```markdown
# Task Handoff

## Goal
[一句话说清楚要实现什么，以及为什么要做]

## Current Understanding
[网页对话阶段已经确认的产品行为、技术方向、边界]

## Context Pointers
[让本地 Agent 优先阅读的文件、目录、已有相似实现]

## Constraints
[禁止行为、代码风格、依赖限制、兼容性要求]

## Out of Scope
[本次明确不做什么]

## Done When
[可验证的验收条件]

## Verification
[希望 Agent 运行的测试、lint、typecheck、build 或手动验证]

## Risks
[你已经知道的风险，让 Agent 重点检查]
```

`Task Handoff` 是任务交接卡，不是完整设计文档。它只保留本地 Agent 开工前必须知道的目标、上下文、边界、验收和风险。

### 字段说明

| 字段 | 中文含义 |
|---|---|
| `Out of Scope` | 本次明确不做什么 |
| `Done When` | 可验证的完成条件 |
| `Verification` | 需要运行或人工检查的验证方式 |
| `Risks` | 已知风险和重点检查点 |

## 为什么要这样写

| 字段 | 解决的问题 |
|---|---|
| `Goal` | 防止 Agent 忘记任务目的 |
| `Current Understanding` | 保留网页对话阶段的判断 |
| `Context Pointers` | 避免 Agent 在全仓库盲搜 |
| `Constraints` | 防止引入无关依赖、重构、风格漂移 |
| `Out of Scope` | 把额外优化挡在外面 |
| `Done When` | 让任务有明确结束线 |
| `Verification` | 要求结果有验证证据 |
| `Risks` | 把人类已知的风险提前交给 Agent |

## 从网页对话提炼交接卡

在网页模型里使用这个提示词：

```text
请把我们刚才讨论的内容整理成一份给本地 coding agent 使用的 Task Handoff。

要求：
- 不要假设它知道我的仓库结构。
- 输出 Goal、Current Understanding、Context Pointers、Constraints、Out of Scope、Done When、Verification、Risks。
- Context Pointers 如果你不知道具体文件，就写“让本地 agent 自己先查找已有实现”。
- Done When（完成条件）必须是可验证的。
- 不要写实现代码，只写任务交接卡。
```

然后把生成结果交给 `Codex`、`Claude Code` 或 `OpenCode`。

## 给本地 Agent 的第一句话

不要在第一条消息中要求它直接实现。先让它读代码。

```text
下面是一份从网页对话整理出的 Task Handoff。请先不要修改文件。

你的第一步：
1. 阅读相关代码和项目规范。
2. 找出现有相似实现。
3. 总结你理解的任务、要改的文件、风险和验证命令。
4. 等我确认后再实施。

[粘贴 Task Handoff]
```

这一步会把网页阶段的“想法”转换成本地阶段的“执行计划”。

## 示例：自动化脚本任务

```markdown
# Task Handoff

## Goal
新增一个脚本，把指定目录下的 Markdown 文件扫描出来，生成包含标题、路径、字数的 CSV 索引。

## Current Understanding
这个脚本用于整理学习资料。用户已经会用终端，不需要 GUI。输出 CSV 后会被表格工具打开。

## Context Pointers
- 先查看项目里是否已有 scripts/ 或 tools/。
- 查找是否已有 CSV 写入、Markdown 解析、文件扫描工具。

## Constraints
- 不要引入新依赖，除非项目已有依赖无法完成。
- 不要修改原 Markdown 文件。
- 默认递归扫描。
- 忽略 .git、node_modules、dist、build。

## Out of Scope
- 不做全文搜索。
- 不做语义分析。
- 不做网页界面。

## Done When
- 用户可以运行一个命令生成 CSV。
- CSV 包含 title、path、word_count 三列。
- 空标题文件也能处理。
- 路径中有空格也能处理。

## Verification
- 用一个临时目录创建 3 个 Markdown 文件验证输出。
- 如果项目有 lint 或 test，运行相关命令。

## Risks
- 中文字数统计可能和英文不同，需要说明统计口径。
- 文件名和路径转义要正确。
```

## 常见错误

### 错误 1：把网页对话全文丢给本地 Agent

问题是噪音太多、决策点不清楚，Agent 可能把讨论过程误当成需求。正确做法是只交付压缩后的任务卡。

### 错误 2：交接卡没有写清 `Out of Scope`

没有写清本次不做什么，Agent 很容易额外改架构、换依赖、重写样式。

### 错误 3：`Done When` 不可验证

这种验收标准没有意义：

```text
功能正常，体验良好，代码优雅。
```

改成：

```text
- `pnpm test` 通过。
- 上传 100 行 CSV 时能展示 3 种错误。
- 数据库写入只在用户确认后发生。
```

## 交接检查表

- [ ] 目标是一句话能说清楚的。
- [ ] 已说明本次不做什么。
- [ ] 已说明本地 Agent 先读哪些地方。
- [ ] 已说明禁止引入哪些变化。
- [ ] 验收条件可以通过命令、diff 或人工操作验证。
- [ ] 风险已提前标出。
