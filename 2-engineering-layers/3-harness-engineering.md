<div data-theme-toc="true"></div>

# 2.3 Harness Engineering：先理解可控系统

Harness Engineering 关心的是 Agent 在什么系统里工作：它先读什么、能改什么、怎么证明完成、失败后从哪里恢复。

本节只讲概念边界。要按步骤搭一个最小 Harness，读 [6.6 为一个项目搭最小 Harness](../6-harness-engineering/6-build-minimal-harness.md)；要直接复制项目模板，读 [9.4 最小项目模板和执行步骤](../9-development-system/4-minimal-template.md)；要找日常执行提示词和检查清单，读 [8. 实用做法](../8-best-practices/index.md)。

## 一个 harness 包含什么

```text
输入：issue / PRD / prompt
上下文：AGENTS.md / CLAUDE.md / docs / specs
工具：shell / Git / MCP / browser / code search
执行：Claude Code / Codex / OpenCode / subagents
验证：test / lint / typecheck / build / e2e
审查：diff 审查 / 代码审查 / 安全审查
恢复：Git / worktree / plan files / task state
沉淀：rules / skills / retrospectives
```

这是一套围绕 Agent 的工程环境。它不等于某个固定工具名，而是把任务入口、上下文、工具、验证、审查、恢复和记忆放到一起。

OpenSpec、Superpowers、GSD、OMC、ECC 和 Trellis 都可以放进这个框架理解：OpenSpec 偏规格，Superpowers 偏流程技能，GSD 偏长任务阶段治理，OMC 偏多 Agent 编排，ECC 偏 Claude Code 能力补齐，Trellis 偏长期任务和记忆骨架。

## 为什么需要 harness

任务变大后，单靠 prompt 常见的问题是：

- 上下文窗口超出限制。
- Agent 忘记测试。
- Agent 改了无关文件。
- 中途失败后无法恢复。
- 同类任务每次都重新教。
- 写代码和审代码混在一起。
- 多个 Agent 互相覆盖。

Harness 的作用是把这些风险提前放进流程里，而不是等 Agent 跑偏后再靠解释补救。

## 最小可用 harness

个人开发者可以从这套开始：

```text
项目根目录：
  AGENTS.md / CLAUDE.md / opencode.json
  README.md

任务目录：
  prd.md
  plan.md

执行工具：
  Claude Code、Codex 或 OpenCode

验证命令：
  test
  lint
  typecheck
  build

复盘：
  每次重复错误沉淀到规范或 skill
```

不要一开始就追求复杂系统。最小 harness 稳定运行之后，再加 MCP、skills、Trellis、CCG、Superpowers。

## 可用 harness 的标志

你可以用下面清单检查：

- 任务入口清楚：每个任务都有目标、范围、验收。
- 上下文可复用：新会话不需要从零解释项目。
- 权限可控：高风险操作需要人工确认。
- 验证默认执行：完成前不会跳过测试。
- 状态可恢复：中断后能从文件继续。
- 经验可沉淀：重复错误会进入规则或 skill。

## Harness 不是越重越好

如果任务很小：

```text
prompt + 本地 Agent + diff 审查
```

即可。

如果任务复杂：

```text
PRD + plan + Agent + MCP + skill + test + 审查 + worktree
```

才需要。

工具复杂度要跟任务复杂度匹配。小任务用重框架，会让维护成本比任务本身还高。
