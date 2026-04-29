<div data-theme-toc="true"></div>

# 2.4 什么时候该升级

不要只因概念完整性而升级。只有当同类问题重复出现，才把 prompt 升级成 context，把 context 升级成 harness。升级的理由应该来自失败记录，不来自工具清单。

## 从 Prompt 升级到 Context

出现这些信号：

- 每次都要重复告诉 AI 项目怎么跑。
- AI 经常忘记测试命令。
- AI 重复使用项目不允许的写法。
- AI 不知道哪些目录不能改。
- AI 多次漏掉同一个同步文件。

升级方式：

```text
把重复提示词写入 AGENTS.md / CLAUDE.md / rules 文件。
```

## 从 Context 升级到 MCP

出现这些信号：

- AI 经常需要查最新文档。
- AI 需要访问 GitHub issue / PR。
- AI 需要读取网页或浏览器状态。
- AI 需要查数据库 schema。
- 你一直在手动复制外部系统内容。

升级方式：

```text
优先接只读 MCP，再考虑写入型 MCP。
```

## 从 Context 升级到 skills

出现这些信号：

- 同一类任务重复做。
- 每次流程都有固定步骤。
- AI 经常漏步骤。
- 你希望输出格式固定。
- 你希望团队共享一套流程卡片。

升级方式：

```text
把稳定流程写成 skill，而不是继续复制长 prompt。
```

## 从单 Agent 升级到多 Agent

出现这些信号：

- 一个 Agent 同时写代码和审查，容易放过自己的问题。
- 任务可以拆成多个文件边界清楚的子任务。
- 你需要多个方案并行探索。
- 你需要一个 Agent 只做 Scout，另一个只做 Builder，第三个只做 Verifier。

升级方式：

```text
先并行调研，再并行实现。
实现前先明确文件所有权。
```

## 从轻流程升级到工作流框架

出现这些信号：

- 长任务经常中断。
- 多工具之间规范不一致。
- 团队不知道怎么统一 AI 工作方式。
- 需要把任务、规范、journal、quality gate 统一管理。
- 需要 Claude / Codex / Gemini 分工。

升级方式：

```text
OpenSpec: 规格、变更、设计、任务先行。
Superpowers: TDD、计划、验证、审查的做法 skills。
GSD: 长任务分阶段执行，降低上下文失效风险。
OMC: Claude Code 场景下的多 Agent 编排。
ECC: skills、memory、安全、验证、学习能力补全。
Trellis: 任务、规范、记忆、跨工具工作区。
CCG: Claude + Codex + Gemini 多模型路由。
```

不要把这些框架放进同一张工具排名。先判断缺规格、缺工程纪律、缺阶段治理、缺多 Agent 编排，还是缺长期项目记忆。

## 一句话判断

```text
偶发问题：改 prompt。
重复问题：写 context。
系统问题：建 harness。
```
