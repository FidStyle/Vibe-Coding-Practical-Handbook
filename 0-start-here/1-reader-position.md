<div data-theme-toc="true"></div>

# 0.1 读者位置：先知道要补什么

本手册面向刚接触 Vibe Coding、还没形成完整方法的人。你可以已经会写代码，也可以只会用网页 AI 做小东西；关键是愿意看结果、查证据，并为 AI 生成的改动负责。

先把读者位置确认清楚，是为了避免把基础缺口误判成模型不够强。不会看测试失败、不会处理 Git diff、不会判断依赖变更时，本地 Agent 仍然能帮你做事，但你要知道风险在哪里，并在练习过程中把这些基础补上。

## 你需要尽快补上的基础

这些能力不要求一开始就熟练。最低目标是看完相关章节后，知道自己卡在哪里，并能按官方文档或项目 README 补到可用。

| 能力 | 最低可用状态 |
|---|---|
| 终端 | 能进入目录、运行命令、看错误输出 |
| Git | 能看 diff、提交、回滚局部改动 |
| 项目结构 | 能读 README、配置文件和目录结构 |
| 依赖管理 | 能安装依赖、理解 lockfile 大概作用 |
| 调试 | 能看 stack trace，知道怎么复现问题 |
| 测试 | 知道测试、lint、typecheck、构建的区别 |

不熟没关系，先带着问题读。等你要进入本地 Agent、MCP 或 harness 时，再回到这张表补短板。会用基础 Vibe Coding 后，很多编程和环境基础会变得更好补，因为你已经知道它们服务于哪一步。

## 本手册面向谁

适合这类读者：

- 刚接触 Vibe Coding，想从网页对话走向真实项目的人。
- 已经用过网页 GPT / Claude / Gemini，但想进入真实项目。
- 用过 Cursor / Trae / Windsurf，但感觉长任务不稳定的人。
- 编程、Git、终端还不熟，但愿意边做边补基础的人。
- 想重点学习 [Codex](https://github.com/openai/codex)、[Claude Code](https://github.com/anthropics/claude-code) 和 [OpenCode](https://github.com/anomalyco/opencode) 共性操作的人。
- 想理解 [MCP](https://github.com/modelcontextprotocol)、[skills](https://agentskills.io/)、`AGENTS.md`、`CLAUDE.md`、[OpenSpec](https://github.com/Fission-AI/OpenSpec)、[Superpowers](https://github.com/obra/superpowers)、[GSD](https://github.com/gsd-build/get-shit-done)、[OMC](https://github.com/Yeachan-Heo/oh-my-claudecode)、[ECC](https://github.com/affaan-m/everything-claude-code)、[Trellis](https://github.com/mindfold-ai/trellis)、[CCG](https://github.com/fengshao1227/ccg-workflow) 如何组合使用的人。

不适合这类读者：

- 只想让 AI 自动做完全部事情的人。
- 不愿意审查 diff 的人。
- 不愿意补项目运行、Git、终端和测试基础的人。
- 不关心测试和验证的人。
- 把“能跑起来”当成唯一质量标准的人。

## 你的角色变化

传统开发里，你主要做：

```mermaid
flowchart LR
  A[理解需求] --> B[写代码]
  B --> C[调试]
  C --> D[提交]
```

Vibe Coding 里，你更多做：

```mermaid
flowchart LR
  A[定义目标] --> B[组织上下文]
  B --> C[设定边界]
  C --> D[委派执行]
  D --> E[验证结果]
  E --> F[沉淀规则]
```

代码仍然重要。只是你不必亲手写每一行，你更常做的是定义目标、控制范围、检查证据，并把反复出现的问题写回项目规则。

## 自测

如果下面 5 个问题你能回答，适合继续读：

1. 这个项目怎么启动？
2. 这个项目怎么跑测试？
3. Git diff 里哪些改动是 AI 误改？
4. 一个需求里哪些部分不能让 AI 自由发挥？
5. AI 做错一次之后，应该把什么沉淀到规范文件？

如果无法回答，不用停在门外。先按第 1 章练任务交接卡，再按 [0.5 开发环境准备](5-environment-setup.md) 补项目运行、Git、终端和测试基础。

## 读完本节的产物

继续往下读前，先写一张自己的起点卡：

```text
我现在能独立运行的项目：
我能跑通的检查命令：
我最不熟的环节：
我准备用来练习的仓库：
我不会交给 Agent 自由处理的高风险区域：
```

这张卡不用正式归档。它的作用是让后面的工具选择有依据，而不是跟着工具清单移动。
