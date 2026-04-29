<div data-theme-toc="true"></div>

# 3.2 Codex 的操作模型

理解 `Codex` 时，可以先把它当成工程执行型 Agent：你给它明确目标、上下文指针、约束和完成标准，它负责在本地仓库里推进任务。

它更适合约束清楚的工程任务：读本地仓库、改文件、跑命令、解释失败，再继续修复，直到结果可验证。

## Codex 最重要的输入

委派给 Codex 的任务，最好把四件事写清楚：

```text
Goal
Context Pointers
Constraints
Done When
```

这也是 Codex 类本地 Agent 最稳定的委派方式。

## AGENTS.md 的作用

`AGENTS.md` 是 Codex 的项目规范入口。不要把它写成团队口号。它应该告诉 Codex：

- 项目如何运行。
- 如何测试。
- 哪些目录重要。
- 哪些规则必须遵守。
- 什么算完成。
- 哪些风险或易错点需要避免。

示例：

```markdown
# AGENTS.md

## Commands
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`
- Build: `pnpm build`

## Workflow
- Before editing, inspect existing implementations.
- For multi-file tasks, propose a plan first.
- Before completion, run relevant checks.

## Guardrails
- Do not change public API shape unless explicitly requested.
- Do not add dependencies without explaining why.
```

## Codex 适合的任务

适合：

- 明确功能。
- 后端逻辑。
- Bug 修复。
- 测试补齐。
- CLI / 脚本。
- 重构。
- 文档同步。
- 多 Agent / worktree 并行任务。
- 代码审查和安全/边界风险检查。
- 需要明确审批、沙盒或权限边界的任务。

不适合：

- 需求完全不清楚。
- 需要大量产品探索但没有边界。
- 没有验收标准的“优化一下”。
- 高风险生产操作。
- 需要切换非 OpenAI 模型的工作流。
- 主要诉求是本地模型或多供应商统一入口。

## Codex 的典型优势和代价

| 维度 | 结论 |
|---|---|
| 代码质量 | 适合有边界的实现、bugfix、测试补齐和审查 |
| 速度 | 复杂任务上可能更慢，但通常更愿意走完整推理和验证 |
| 工具链 | 和 OpenAI、ChatGPT、Codex App / IDE / CLI、AGENTS.md、MCP、skills、subagents 等能力放在同一套工作流里 |
| 灵活性 | 官方主线绑定 OpenAI 模型，模型选择自由度不如 OpenCode |
| 安全 | 适合通过 sandbox、approval、最小权限和验证命令控制风险 |

把 Codex 当成“工程执行同事”，而不是开放式讨论对象。任务越明确，它越有价值；边界越模糊，它越容易补充未确认需求。

## 推荐操作流程

```text
1. 用网页或人脑整理 Task Brief。
2. 让 Codex 先只读分析。
3. 要求 Codex 写计划和验证命令。
4. 确认计划。
5. 让 Codex 实现。
6. 让 Codex 运行验证。
7. 人审 diff。
8. 让 Codex 根据审查结果修复。
```

## Codex 使用提醒

- 不要省略 `Done When`。
- 不要让 Codex 在没有上下文指针时猜。
- 不要让多个 Agent 改同一个文件范围。
- 不要把高风险写入权限直接交给它。
- 不要把“运行失败但推测没问题”当完成。
- 不要用它替代代码审查。让它先自查，再由人审 diff。
- 如果你需要多供应商或本地模型，把 OpenCode 放到工具链里。
