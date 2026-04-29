<div data-theme-toc="true"></div>

# 4.1 Editor-first vs TUI-first

AI 编程工具大致分两类。区别不在于谁更“高级”，而在于谁主导执行：

| 类型 | 代表 | 工作方式 |
|---|---|---|
| Editor-first | Cursor、Windsurf、Trae | 人在编辑器里主导，AI 做补全、局部修改、上下文辅助 |
| TUI-first | [Claude Code](https://github.com/anthropics/claude-code)、[Codex](https://github.com/openai/codex)、[OpenCode](https://github.com/anomalyco/opencode)，[Gemini CLI](https://github.com/google-gemini/gemini-cli) 辅助 | 人定义任务，Agent 在终端中读写文件、跑命令、管理验证 |

## Editor-first 适合什么

适合：

- 局部代码补全。
- 单文件修改。
- 边看边改。
- 快速解释代码。
- UI 小调整。
- 不想离开 IDE 的开发者。

长处：

- 上手快。
- 视觉反馈直接。
- 和编辑器操作习惯一致。
- 适合短周期任务。

## TUI-first 适合什么

适合：

- 多文件任务。
- Bug 定位。
- 重构。
- 跑测试和构建。
- 长任务。
- 多 Agent。
- MCP / skills / hooks / worktree。
- 把状态写入文件。

长处：

- 更接近工程流水线。
- 更容易形成可恢复任务。
- 更适合严格验证。
- 更容易接入脚本和自动化。

主力 TUI 工具的分工：

| 工具 | 更适合 |
|---|---|
| Claude Code | 复杂任务、skills / hooks / subagents |
| Codex | 明确委派、测试修复、审查、OpenAI 工具链 |
| OpenCode | 开源可控、多模型、本地模型、LSP、自定义 agent |
| Gemini CLI | 低成本、长上下文阅读、资料压缩和第二意见 |

## 关键差异

| 问题 | Editor-first | TUI-first |
|---|---|---|
| 谁主导 | 人在 IDE 中主导 | 人定义任务，Agent 执行 |
| 任务跨度 | 局部到中等 | 中等到复杂 |
| 验证方式 | 人经常手动跑 | Agent 可被要求默认跑 |
| 上下文 | 编辑器上下文 | 文件系统 + 规范 + 工具 |
| 长任务 | 容易漂 | 可用文件和任务系统承载 |
| 扩展 | IDE 插件 | MCP / skills / hooks / CLI |

## 推荐组合

不是二选一。编辑器工具负责局部协助，TUI Agent 负责把一个任务从计划推到验证。

```text
编辑器：
  看代码、人工编辑、局部补全、diff 审查。

TUI Agent：
  任务执行、计划、验证、复盘。
```

真实工作流里，两者可以并存。你可以在编辑器里审查 diff，也可以让 TUI Agent 负责跑完整任务。
