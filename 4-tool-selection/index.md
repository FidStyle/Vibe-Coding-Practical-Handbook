<div data-theme-toc="true"></div>

# 4. Coding 工具选型：本地 TUI Agent 与编辑器 Agent

本章回答一个现实问题：已经有 `Cursor`、`Trae`、`Windsurf`，为什么还要学 `Claude Code` / `Codex` / `OpenCode`？

答案取决于工作形态。有些工具适合你坐在编辑器里边看边改，有些工具适合把任务交出去，让它读仓库、改文件、跑验证。

本章是并列选型章节。不要按顺序把所有工具都学一遍，按任务形态选择就够了。

## 本章结构

| 子章节 | 目标 |
|---|---|
| [1-editor-vs-tui.md](1-editor-vs-tui.md) | 区分 Editor-first 和 TUI-first |
| [2-cursor-trae-windsurf.md](2-cursor-trae-windsurf.md) | 简要说明 Cursor / Trae / Windsurf 的位置 |
| [3-selection-matrix.md](3-selection-matrix.md) | 给出任务选型矩阵 |
| [4-scenario-playbook.md](4-scenario-playbook.md) | 按真实场景选择工具 |

## 本章阅读方式

```text
先区分 Editor-first / TUI-first。
再理解 Cursor / Trae / Windsurf 的位置。
最后用矩阵和场景指南做实际选择。
```

工具选择要落到任务上：小范围补全优先编辑器，大范围修改优先本地 Agent；需要外部事实时接 MCP，需要重复流程时写 skill；需要跨会话、跨角色和跨验证时，才进入 harness。

## 读完要学会什么

| 产物 | 用途 |
|---|---|
| Editor-first / TUI-first 判断 | 决定任务是在编辑器里边看边改，还是交给本地 Agent |
| 工具缺口矩阵 | 按任务缺口选择工具 |
| 场景选型记录 | 说明为什么这次用某个工具 |

## 本章边界

本章只做选型，不讲 MCP 配置、skill 编写和 harness 搭建。需要扩展 Agent 能力时进入 [5. MCP 与 skills](../5-mcp-skills/index.md)；需要治理长任务时进入 [6. Harness Engineering](../6-harness-engineering/index.md)。
