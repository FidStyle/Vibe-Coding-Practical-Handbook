<div data-theme-toc="true"></div>

# 3.3 Claude Code 的操作模型

`Claude Code` 可以理解为一个交互强、上下文能力强、扩展点多的终端开发 Agent。它适合陪你把复杂任务拆开，而不是只当一个会写代码的输入框。

## Claude Code 的强项

- 阅读复杂代码和长文档。
- 多文件任务规划。
- 交互式调试。
- `CLAUDE.md` 规则注入。
- MCP、hooks、skills、subagents 等扩展点。
- 把任务拆成 Research / Plan / Implement / Validate。
- 把重复工作流写成可触发能力，而不是每次复制长 prompt。

## Claude Code 的典型优势和代价

| 维度 | 结论 |
|---|---|
| 使用入口 | 适合直接进入日常开发、长任务和复杂代码库理解 |
| 扩展方式 | `CLAUDE.md`、skills、MCP、hooks、subagents 的组合路径清晰 |
| 交互方式 | 适合多轮规划、实时纠偏、中文需求整理和复杂任务拆分 |
| 灵活性 | 官方主线绑定 Claude 工具链，不是多供应商 TUI |
| 成本和额度 | 重度使用时要关注套餐、额度和团队合规要求 |

Claude Code 的关键在分层使用：常驻规则放 `CLAUDE.md`，重复流程放 skill，外部系统放 MCP，强制动作放 hook，隔离任务放 subagent。拆清楚之后，Agent 不容易把所有问题都挤进同一段长 prompt。

## CLAUDE.md 的作用

`CLAUDE.md` 是 Claude Code 常见的项目规范文件。它应该简短、明确、具体，最好像工作说明书，不要像口号。

应该写：

```markdown
## Commands
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`

## Workflow
- Research before editing.
- For large tasks, write a plan first.
- Run checks before saying done.

## Project Rules
- API errors use `src/lib/api-error.ts`.
- Auth changes must update session types.
```

不应该写：

```markdown
- Write clean code.
- Follow general quality rules.
- Be careful.
```

这些无法验证。

## Plan Mode / Thinking Mode 的意义

复杂任务不要直接让 Claude Code 写。先让它列出结构、文件范围和风险，再让它动手。

更稳的方式：

```text
先进入规划：
  让它读代码、列文件、列风险、列验证。

再进入实现：
  按计划逐步改。

最后进入验证：
  跑测试、修复、总结 diff。
```

如果上下文变长，用文件传递状态，而不是继续追加聊天记录。聊天记录越长，旧目标越容易影响当前目标。

## Claude Code 适合的任务

适合：

- 大代码库理解。
- 跨文件功能。
- 前后端整合。
- 复杂 bug。
- 需要 MCP 或浏览器工具的任务。
- 需要子 Agent 做探索或并行方案的任务。
- 需要把团队流程写成 skills / hooks 的任务。

不适合：

- 没有边界的自由发挥。
- 没有测试的高风险修改。
- 完全交给它“自动上线”。
- 明确需要接多个非 Claude 模型的工作流。
- 主要诉求是本地模型或开源可改造外壳。

## 使用提醒

- 经常要求它先只读分析。
- 大任务用文件保存计划。
- 不要让一个会话混合太多目标。
- 上下文过长时主动阶段切换。
- 把反复犯错沉淀到 `CLAUDE.md` 或 skill。
- hook 适合强制规则，skill 适合可推理流程，先分清再组合。
- subagent 适合隔离上下文，不适合把模糊需求拆给多个 Agent 并行推进。
