<div data-theme-toc="true"></div>

# 5.1 MCP 入门：先接只读工具

[MCP](https://github.com/modelcontextprotocol) 是 `Model Context Protocol`。可以把它理解为 Agent 调用外部工具和数据的标准接口。

一次典型调用很简单：AI 应用启动或连接一个 MCP server，列出可用工具和资料；模型决定需要工具时，客户端把参数发给 server；server 返回结果；模型再继续分析。你要关心的是四件事：它能读什么、能改什么、凭据在哪里、出错后能不能撤销。

## MCP 三类能力

| 能力 | 含义 | 例子 |
|---|---|---|
| Tools | Agent 可主动调用的函数 | 搜索代码、查文档、操作浏览器 |
| Resources | Agent 可读取的上下文 | 文件列表、数据库 schema、文档 |
| Prompts | 预设交互模板 | 审查模板、发布模板 |

多数开发者一开始只需要 Tools。先把只读工具跑通，再考虑带写入权限的工具。不要把 MCP 当普通插件看，它拿到的权限往往接近你本机账号的权限。

## 先接什么

不要一次接入过多 MCP。通常按以下顺序验证：

```text
1. 文档检索 MCP
2. 代码搜索 MCP
3. 浏览器 / Playwright MCP
4. GitHub / GitLab MCP
5. Jira / Linear / Notion MCP
6. 内部系统 MCP
```

第一次挑实现时，可以先看只读或低风险项目：

- [OpenAI Docs MCP](https://platform.openai.com/docs/docs-mcp)：查 OpenAI / Codex / API 文档。
- [Filesystem reference server](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)：只指向一个练习目录。
- [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)：用隔离浏览器调试页面。
- [Playwright MCP](https://github.com/microsoft/playwright-mcp)：复现网页流程并生成测试。
- [GitHub MCP Server](https://github.com/github/github-mcp-server)：读 issue、PR、仓库信息。
- [Notion MCP](https://developers.notion.com/guides/mcp/overview)：连接文档工作区，注意它可能有写权限。

`modelcontextprotocol/servers` 里的 reference servers 主要用于学习和参考，不等于生产级服务。正式接入前要读 README、权限说明和维护状态。

接入顺序：

- 先只读。
- 后写入。
- 先低权限。
- 后高权限。

## MCP 什么时候有价值

适合：

- 查最新框架文档。
- 读取 issue / PR。
- 复现浏览器交互。
- 查数据库 schema。
- 读取内部知识库。
- 减少复制粘贴。

不适合：

- 一个 shell 命令就能完成的事。
- 权限边界不清楚的事。
- 输出巨大、污染上下文的工具。
- 不稳定、经常断的工具。

## MCP 安全边界

默认配置：

```text
只读。
限定目录。
限定域名。
限制输出长度。
高风险写入需要人工确认。
```

任何能写文件、改数据库、发请求、改工单状态、操作远端仓库的 MCP，都应该按高风险工具处理。

接入前至少问七个问题：

| 问题 | 检查点 |
|---|---|
| 读写边界 | 它到底需要读什么，是否真的需要写权限 |
| 权限范围 | 能否限制到项目子目录、测试库或隔离账号 |
| 网络行为 | 会访问哪些域名，是否把代码或日志发到外部 |
| 凭据距离 | 是否能读取 `.env`、shell history、云账号配置 |
| 日志审计 | 调用了什么工具、参数是什么、结果能否追踪 |
| 重试副作用 | 失败后重复调用是否会重复写入、重复发送或重复删除 |
| 供应链 | 作者、维护状态、安装脚本和依赖是否可信 |

## 验证 MCP 是否可用

一个 MCP 接入后，必须验证：

- Agent 能看到工具。
- 工具能成功调用。
- 失败时能看到错误信息。
- 输出足够短。
- 权限范围符合预期。

不要一次接入多个 MCP。先验证一个，再接入下一个。每次验证都要留下工具名、权限范围和失败处理方式。

## 三个入门练习

### 练习 1：只读文档 MCP

Codex：

```bash
codex mcp add openaiDeveloperDocs --url https://developers.openai.com/mcp
codex mcp list
```

Claude Code：

```bash
claude mcp add --transport http openaiDeveloperDocs https://developers.openai.com/mcp
claude mcp list
```

OpenCode：

```bash
opencode mcp add
opencode mcp list
```

`opencode mcp add` 会进入交互式配置。也可以直接写 `opencode.json`，适合团队把只读 MCP 放进项目配置：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "openaiDeveloperDocs": {
      "type": "remote",
      "url": "https://developers.openai.com/mcp",
      "enabled": true
    }
  }
}
```

练习提示词：

```text
Use the OpenAI developer docs MCP server to look up the current Codex MCP setup command. Summarize the answer and include the source link.
```

### 练习 2：沙盒文件夹

先建一个空目录，不要把 MCP 指向整个 home 目录或公司仓库：

```bash
mkdir -p ~/mcp-sandbox
claude mcp add filesystem --scope local -- npx -y @modelcontextprotocol/server-filesystem ~/mcp-sandbox
```

OpenCode 可以用交互式命令添加，也可以把本地 server 写进 `opencode.json`。示例里的路径要换成你的绝对路径：

```bash
opencode mcp add
```

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "filesystem-sandbox": {
      "type": "local",
      "command": [
        "npx",
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/absolute/path/to/mcp-sandbox"
      ],
      "enabled": true
    }
  }
}
```

Windows 上，Claude Code 调 `npx` 有时需要 `cmd /c`：

```bash
claude mcp add filesystem -- cmd /c npx -y @modelcontextprotocol/server-filesystem C:\path\to\mcp-sandbox
```

OpenCode 在 Windows 上也可以把 `cmd /c` 放进配置：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "filesystem-sandbox": {
      "type": "local",
      "command": [
        "cmd",
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\path\\to\\mcp-sandbox"
      ],
      "enabled": true
    }
  }
}
```

练习提示词：

```text
In the sandbox folder only, create a note called mcp-test.md with three MCP safety lessons. Do not touch files outside that folder.
```

### 练习 3：隔离浏览器

配置 Chrome DevTools MCP 时，用隔离 profile：

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest",
        "--isolated",
        "--no-usage-statistics"
      ]
    }
  }
}
```

练习提示词：

```text
Open http://localhost:3000 in an isolated browser. Check console errors, failed network requests, and one visible layout problem. Report observations only; do not edit files yet.
```

不要用已经登录邮箱、网银、公司后台或生产控制台的浏览器 profile 做练习。

如果 MCP 使用 HTTP 传输，安全要求更高：使用 HTTPS；token 放在 header，不放在 URL；OAuth 回调只接受 `localhost` 或 HTTPS 地址；生产写操作必须有人类确认点。
