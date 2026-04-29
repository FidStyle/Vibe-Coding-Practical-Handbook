<div data-theme-toc="true"></div>

# 0. 起步与定位

不要从工具名单开始。本章先确认三件事：你是不是这本手册的目标读者，本书里的 Vibe Coding 指什么，以及接下来按什么顺序补能力。

如果你刚接触 Vibe Coding，不确定网页对话、本地 Agent、MCP、skills 和 harness 各自解决什么问题，先读这一部分。多数问题不在模型名，而在任务没有写清、上下文没有落盘、验证没有进入默认流程。

这里是全书入口，不是工具安装页。读完后，你应该能判断自己当前缺的是 prompt、context、MCP / skills，还是 harness。

## 本章结构

| 子章节 | 目标 |
|---|---|
| [1-reader-position.md](1-reader-position.md) | 确认你是不是本手册的目标读者 |
| [2-what-vibe-coding-means.md](2-what-vibe-coding-means.md) | 定义本手册里的 Vibe Coding |
| [3-learning-path.md](3-learning-path.md) | 给出推荐学习路线 |
| [4-mental-model-glossary.md](4-mental-model-glossary.md) | 术语和心智模型地图 |
| [5-environment-setup.md](5-environment-setup.md) | 按 Windows / macOS / Linux / WSL 准备开发环境 |

## 本章阅读方式

```mermaid
flowchart LR
  A[读者定位] --> B[Vibe Coding 定义]
  B --> C[学习路径]
  C --> D[术语地图]
  D --> E[环境准备]
```

## 读完要学会什么

| 产物 | 用途 |
|---|---|
| 读者起点卡 | 判断自己该从哪一层开始补 |
| Vibe Coding 定义 | 避免把“AI 写代码片段”误当成完整开发 |
| 学习路线 | 按阶段补任务、上下文、工具和系统 |
| 术语地图 | 后文出现 Prompt、Context、MCP、skills、Harness 时不混淆 |
| 环境检查结果 | 确认本地 Agent 能读仓库、装依赖、跑命令 |

## 本章边界

这本手册不把编程基础、Git、终端和 Linux 当成入场证。会这些会更顺；不会也可以先读到“我缺哪一块、为什么要补、补到什么程度”。本章会把基础缺口摊开，后面的章节再训练 AI 参与开发后的工程问题：任务如何落到文件，上下文如何长期保存，Agent 如何验证，失败后如何接回来。

网页对话、本地 Agent、MCP、[skills](https://agentskills.io/) 和 Harness 不是同一类东西。网页对话适合澄清想法，本地 Agent 负责读仓库和改文件，MCP 接事实和工具，skills 固化流程，Harness 把这些能力放进可恢复的项目系统。

读完本章后进入 [1. 从网页对话进入 Vibe Coding](../1-web-chat/index.md)。如果你已经能把需求写成清楚任务卡，可以直接跳到 [3. 本地 Agent](../3-local-agents/index.md)。
