<div data-theme-toc="true"></div>

# 0.5 开发环境准备：按系统走，不要硬凑命令

AI 编程工具不是只靠模型。它们要读文件、跑命令、装依赖、执行测试，还要处理 Git diff。环境不稳，Agent 再聪明也会卡在安装、编码、路径和权限问题上。

本节给一条较短的路线：先确认 Git、Node、npm 和 uv 可用，再进入 Codex、Claude Code、OpenCode、Gemini CLI、MCP 和 skills。

这里不追求把每个系统配置到完美。目标是让本地 Agent 能稳定读仓库、装依赖、跑检查，并且你能看懂失败原因。

如果你还不熟终端、Git、Node 或 Python，不需要先补成开发老手。先把本节的检查命令跑通，知道失败时该查哪个工具，再进入后面的本地 Agent 练习。

## 先选系统路线

| 系统 | 推荐路线 | 适合谁 |
|---|---|---|
| Windows | 优先 WSL 2 + Ubuntu，必要时再用原生 PowerShell | 需要跑本地 Agent、Node、Python、Docker、shell 脚本的人 |
| Windows 原生 | PowerShell + `nvm-windows` + `uv` | 暂时没有 Linux 环境、只想先跑通基础练习的人 |
| macOS | Terminal / iTerm2 + Homebrew + `nvm` + `uv` | 大多数个人开发者 |
| Linux | 发行版包管理器 + `nvm` + `uv` | 已经熟悉终端和权限的人 |

Windows 原生环境可以做开发，但初学 AI Agent 时更容易遇到这些问题：GBK/UTF-8 编码混乱、PowerShell 和 Bash 语法不同、路径格式不同、删除或移动文件时更难判断影响范围。WSL 让命令更接近多数开源项目、CI 和服务器环境，适合作为本手册的默认 Windows 路线。

如果你还没有 Linux 环境，Windows 原生路线可以先用来完成基础练习。等要跑更复杂的本地 Agent、shell 脚本、Docker 或项目自动化时，再切到 WSL 或 Linux 会更少绕路。

本手册的 WSL 示例默认使用 Ubuntu。WSL 安装步骤按微软官方文档执行即可：[Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install) 和 [Set up a WSL development environment](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)。

如果你用 WSL，项目尽量放在 Linux 文件系统里：

```bash
mkdir -p ~/code
cd ~/code
```

## 系统基础工具

Git 只需要确认已安装。后面的 `commit`、`diff` 和审查流程会用到它；如果 `git --version` 失败，先按 Git 官网或系统自带渠道装好 Git，再回到本节。这里不展开安装步骤和身份配置。

先确认 Git 可用：

```bash
git --version
```

## Node：按系统选择 nvm

大多数前端和 AI 编程工具需要 Node。macOS、Linux、WSL 推荐 [`nvm`](https://github.com/nvm-sh/nvm)；按 GitHub README 的当前说明安装即可。安装后重开终端，确认 `nvm` 可用，再装 Node 22：

```bash
nvm --version
nvm install 22
nvm use 22
node -v
npm -v
```

Windows 原生 Node 适合暂时没有 Linux 环境的读者。先安装 [`nvm-windows`](https://github.com/coreybutler/nvm-windows)，重新打开 PowerShell 后执行：

```powershell
nvm --version
nvm install 22
nvm use 22
node -v
npm -v
```

## npm 镜像

优先知道当前 registry 是什么：

```bash
npm config get registry
npm config set registry https://registry.npmjs.org/
```

国内网络不稳定时，可以临时切到 npmmirror：

```bash
npm config set registry https://registry.npmmirror.com
npm config get registry
```

恢复官方源：

```bash
npm config set registry https://registry.npmjs.org/
```

镜像只用于访问不稳定时的临时加速。平时能访问官方源，就用官方源。

## Python：优先用 uv

`uv` 的安装方式按官方文档执行：[uv installation](https://docs.astral.sh/uv/getting-started/installation/)。安装后先确认命令可用：

```bash
uv --version
```

在项目目录里创建虚拟环境：

```bash
uv venv
```

激活虚拟环境。macOS / Linux / WSL：

```bash
source .venv/bin/activate
```

Windows PowerShell：

```powershell
.venv\Scripts\Activate.ps1
```

临时使用清华 PyPI 镜像：

```bash
uv pip install -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple <package>
```

## GitHub 国内访问：只讲合规办法

有些海外站点和 IP 在国内访问不稳定。不要把时间耗在不可审查的绕路教程上。更稳的做法是：

- 优先访问项目官网、官方文档和官方发布页。
- GitHub 下载慢时，可以使用 [Watt Toolkit](https://steampp.net/download) 这类加速工具。
- 如果只是读代码或下载开源项目，先查 Gitee / GitCode 是否有作者维护的镜像。
- 自己的仓库可以使用 Gitee / GitCode 的导入或镜像同步功能，但要先读平台说明，避免覆盖分支、标签或漏掉 Git LFS。
- 提交 PR、报告安全问题、确认 release 时，仍以项目上游官方仓库为准。

## 模型和 API 访问

本地 Agent 练习开始前，先准备一个可用的模型或 API 来源。不要等到第一次任务会话里再处理额度、API key、地区访问和 provider 配置问题。

| 来源 | 获取位置 | 常见用途 |
|---|---|---|
| OpenAI | [OpenAI Platform API keys](https://platform.openai.com/api-keys) | Codex、OpenAI API、OpenAI 兼容工具 |
| Anthropic | [Anthropic Console](https://console.anthropic.com/settings/keys) | Claude Code、Claude API |
| DeepSeek | [DeepSeek API docs](https://api-docs.deepseek.com/) | DeepSeek API、OpenAI 兼容调用 |
| 智谱 GLM / BigModel | [智谱开放平台文档](https://docs.bigmodel.cn/) | GLM API、国内模型接入 |

API key 不要写进公开项目文件。优先放在环境变量、用户级配置，或工具官方支持的配置文件里。

常见 provider 配置位置：

| 工具 | 配置位置 |
|---|---|
| Codex | `~/.codex/config.toml` |
| Claude Code | `~/.claude/settings.json` |
| OpenCode | `~/.config/opencode/opencode.json`，或项目根目录 `opencode.json` |

如果你经常在多个 provider 之间切换，可以了解 [cc-switch](https://github.com/farion1231/cc-switch)。它只作为可选的切换辅助工具出现，本手册不要求安装。

## 验证清单

Bash / macOS / Linux / WSL：

```bash
git --version
nvm --version
node -v
npm -v
uv --version
```

PowerShell：

```powershell
git --version
nvm --version
node -v
npm -v
uv --version
```

这些命令都能跑通，再进入本地 Agent 安装。否则 Agent 很可能把时间花在修环境，而不是修项目。

## 常见停止点

遇到下面情况，先停下来修环境，不要继续让 Agent 猜：

- `git --version` 不可用。
- `node -v` 和项目要求版本差异很大。
- `npm install` 因权限、路径或编码问题失败。
- `uv --version` 不可用，但项目需要 Python 工具。
- 项目放在 WSL 的 `/mnt/c` 或 `/mnt/d` 下，文件扫描明显变慢。
- 终端输出乱码，无法判断错误信息。

环境问题要记录在任务说明里。否则下一次 Agent 仍会在同一个地方失败。
