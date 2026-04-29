<div data-theme-toc="true"></div>

# 99. 公开资料与延伸阅读

这一页只放继续查阅的公开资料方向。工具版本会变，读的时候以官方文档和仓库现状为准。

这不是正文必读章节。遇到工具版本、安装方式、权限配置或官方能力变化时，再回到这里查公开入口。

## 官方文档

| 主题 | 建议查阅内容 |
|---|---|
| [Codex](https://github.com/openai/codex) | 官方文档中的 prompting、`AGENTS.md`、security、config、CLI / IDE / App 使用说明 |
| [Claude Code](https://github.com/anthropics/claude-code) | 官方文档中的 quickstart、memory、`CLAUDE.md`、MCP、hooks、skills、subagents |
| [OpenCode](https://github.com/anomalyco/opencode) | 官方文档中的 CLI、agents、providers、LSP、MCP、skills、permissions、configuration |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | 官方文档中的安装、长上下文、MCP、用量限制和配置 |
| [Model Context Protocol](https://github.com/modelcontextprotocol) | MCP concepts、tools / resources / prompts、server quickstart、client integration |
| [Agent Skills](https://agentskills.io/) | Agent Skills 标准、结构和示例；源码和贡献入口见 [agentskills/agentskills](https://github.com/agentskills/agentskills) |
| [OpenAI Docs MCP](https://platform.openai.com/docs/docs-mcp) | Codex / VS Code / Cursor 里接入 OpenAI 官方文档 MCP |
| [Claude Code MCP](https://docs.anthropic.com/en/docs/claude-code/mcp) | MCP server 作用域、`.mcp.json`、OAuth、Windows `cmd /c npx` 注意事项 |
| [Claude Code skills](https://docs.claude.com/en/docs/claude-code/skills) | skill 目录、`SKILL.md`、触发方式、支持文件和限制 |
| GitHub / [GitLab](https://github.com/gitlabhq/gitlabhq) | issue、PR、审查、CI、权限和 token 管理 |
| [Playwright MCP](https://github.com/microsoft/playwright-mcp) / [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp) | 浏览器检查、交互复现、测试生成、性能和 console/network 调试 |
| [uv](https://docs.astral.sh/uv/) / [nvm](https://github.com/nvm-sh/nvm) / [Node.js](https://nodejs.org/en/download) | Python、Node 和包管理器安装 |
| [Microsoft WSL](https://learn.microsoft.com/en-us/windows/wsl/install) | Windows 上使用 Linux 开发环境 |
| [Watt Toolkit](https://steampp.net/download) | GitHub 下载慢时的加速工具入口 |
| [DeepSeek](https://chat.deepseek.com/) / [Kimi](https://kimi.com/) / [通义千问](https://qianwen.aliyun.com/) / [豆包](https://www.doubao.com/product) | 海外工具不可用时的国内网页端替代 |

## 推荐学习主题

| 阶段 | 继续学习什么 |
|---|---|
| 网页对话 | 需求澄清、PRD、任务交接卡、模型选择 |
| Prompt Engineering | 目标、上下文、约束、验收标准 |
| Context Engineering | `AGENTS.md`、`CLAUDE.md`、项目文档、任务目录、上下文压缩 |
| 本地 TUI Agent | 只读分析、计划、最小 diff、验证、审查、恢复 |
| MCP / skills | 只读工具、权限分级、skill 触发条件、工具与流程组合 |
| Harness Engineering | 任务治理、多 Agent、worktree、CI、评测、复盘和记忆 |

## 阅读资料时怎么判断

- 优先看官方文档和一手工程实践。
- 先确认资料对应的工具版本和发布日期。
- 区分“演示效果”和“真实项目工作流”。
- 只把能复用、能验证、能降低风险的内容写进自己的项目规则。
- 对高权限 MCP、自动写入、自动部署类方案保持默认谨慎。

## 可以整理成自己的资料

- 常用任务交接卡模板。
- `AGENTS.md` / `CLAUDE.md` 项目模板。
- PRD、计划、日志、验证模板。
- PR 审查和 bugfix skills。
- MCP 接入清单和权限分级表。
- 个人或团队的最终验收清单。
