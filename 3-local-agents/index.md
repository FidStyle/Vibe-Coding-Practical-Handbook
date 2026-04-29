<div data-theme-toc="true"></div>

# 3. 本地 Agent：Claude Code / Codex / OpenCode

这一模块是全书主线。你不需要把 [Claude Code](https://github.com/anthropics/claude-code)、[Codex](https://github.com/openai/codex) 和 [OpenCode](https://github.com/anomalyco/opencode) 当成三个世界。换工具会改变权限、模型选择和扩展方式，但日常开发要练的仍是同一条 Agent Loop：读代码、建计划、改文件、跑验证、审 diff、沉淀规则。

## 本章结构

先看定位，再跑同一条 loop。

### 工具定位

| 子章节 | 目标 |
|---|---|
| [1-why-local-agents.md](1-why-local-agents.md) | 理解本地 Agent 的价值 |
| [2-codex-operating-model.md](2-codex-operating-model.md) | 理解 Codex 的操作模型 |
| [3-claude-code-operating-model.md](3-claude-code-operating-model.md) | 理解 Claude Code 的操作模型 |
| [4-opencode-operating-model.md](4-opencode-operating-model.md) | 理解 OpenCode 的操作模型 |

### 共通操作

| 子章节 | 目标 |
|---|---|
| [5-common-agent-loop.md](5-common-agent-loop.md) | 掌握共通 Agent Loop（代理工作循环） |
| [6-prompt-templates.md](6-prompt-templates.md) | 复用任务模板 |
| [7-first-session-walkthrough.md](7-first-session-walkthrough.md) | 跑通第一次真实任务会话 |
| [8-debugging-and-recovery.md](8-debugging-and-recovery.md) | 处理 Agent 跑偏和失败恢复 |

编号 3.4 留给 OpenCode 的操作模型，所以共通操作从 3.5 开始。第一次上手可以按 3.5 -> 3.7 读；已经熟悉本地 Agent 的读者，可以直接拿 3.6 的任务模板开工。

### 选型

| 子章节 | 目标 |
|---|---|
| [9-cli-comparison-and-selection.md](9-cli-comparison-and-selection.md) | 按约束选择 CLI Agent |

## 主要结论

`Claude Code`、`Codex` 和 `OpenCode` 具体长得不一样，但决定成败的是这条循环：

```mermaid
flowchart LR
  A[读代码] --> B[建计划]
  B --> C[改文件]
  C --> D[跑验证]
  D --> E[看 diff]
  E --> F[修复]
  F --> G[记录经验]
```

能把这条循环跑稳，才算会用本地 Agent。只会换模型名字，解决不了工程问题。

开始安装 CLI 前，先按 [0.5 开发环境准备](../0-start-here/5-environment-setup.md) 检查 Git、Node、Python/uv、模型访问和系统路线。Windows 用户如果没有明确理由使用原生 PowerShell，优先在 WSL 里练本地 Agent。

本章只讲本地 Agent 怎么工作。工具之间的取舍放在 [3.9 CLI Agent 对比与选型](9-cli-comparison-and-selection.md)，编辑器 Agent 和 TUI Agent 的边界放在 [4. Coding 工具选型](../4-tool-selection/index.md)。

## 读完要学会什么

| 产物 | 用途 |
|---|---|
| 一个可用本地 Agent | 能在真实仓库里读文件、改文件、跑命令 |
| Agent Loop | 每次任务按只读分析、计划、实现、验证、审查、记录推进 |
| 任务模板 | 把常见任务交给 Agent 时少重写提示词 |
| 第一次任务记录 | 留下 diff、验证命令和复盘 |
| 恢复流程 | Agent 跑偏时知道先看 diff、再收缩任务 |

## 本章边界

本章关注本地执行闭环，不把 MCP、skills 和 harness 一次性塞进第一次任务。读完本章后进入 [4. Coding 工具选型](../4-tool-selection/index.md) 和 [5. MCP 与 skills](../5-mcp-skills/index.md)。如果你已经知道用哪个 Agent，可以直接读 [3.5 共通 Agent Loop](5-common-agent-loop.md)。
