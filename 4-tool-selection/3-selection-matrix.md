<div data-theme-toc="true"></div>

# 4.3 按任务缺口选工具

Coding 工具选型应该从任务出发。先问“当前缺什么能力”，再问“哪个工具能补这一层”。

## 快速矩阵：缺口到工具

| 当前缺口 | 可选工具 |
|---|---|
| 概念澄清、方案比较 | GPT / Claude / Gemini 网页 |
| 把想法整理成 PRD 草稿 | Claude / GPT 网页 |
| 阅读较长资料并提取问题 | Gemini / Claude |
| 单文件小改，人工持续盯着 diff | Cursor / Windsurf / Trae / Codex / Claude Code / OpenCode |
| 多文件功能，需要 Agent 读仓库和跑验证 | [Claude Code](https://github.com/anthropics/claude-code) / [Codex](https://github.com/openai/codex) / [OpenCode](https://github.com/anomalyco/opencode) |
| bug 定位、测试补齐、重构回收 | Claude Code / Codex / OpenCode |
| 前端视觉、CSS、交互细节 | Claude Code / Gemini / Cursor，再配合浏览器检查 |
| 多模型或本地模型工作流 | OpenCode |
| 外部系统读取、浏览器复现、文档检索 | Claude Code / Codex / OpenCode + MCP |
| 需求、长任务、多 Agent 或团队规范开始失控 | 进入 [6. Harness Engineering](../6-harness-engineering/index.md) |

## 三个判断问题

### 1. 任务需要真实仓库吗？

不需要：

```text
网页模型即可。
```

需要：

```text
本地 Agent 或 AI IDE。
```

### 2. 任务需要跑验证吗？

不需要：

```text
网页或编辑器都可以。
```

需要：

```text
优先 Claude Code / Codex / OpenCode。
```

### 3. 任务会反复出现吗？

不会：

```text
写好 prompt 即可。
```

会：

```text
写入 AGENTS.md / CLAUDE.md / agent config / skill / harness。
```

## 最小推荐组合

个人开发者先配置一套满足基本需求的工具，不需要一次性配置完整工具链：

```text
VS Code 或 Cursor
Claude Code / Codex / OpenCode 任选一个主力
Gemini CLI 辅助
1 个文档检索 MCP
1 个自定义 bug-fix skill
```

团队更需要把规则、验证和协作边界放进去：

```text
Claude Code / Codex / OpenCode
AGENTS.md / CLAUDE.md / agent config
MCP 接入团队工具
CI 验证
代码审查 skill
任务目录或 specs 目录
```

## 选型要留下的记录

每次引入新工具前，写下四行即可：

```text
要补的缺口：
不用它的代价：
权限和副作用：
如何验证它确实有用：
```

说不清这四行时，先不要安装。工具没有对应缺口，只会增加维护面。
