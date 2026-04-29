<div data-theme-toc="true"></div>

# 1.3 把网页聊天变成本地任务

网页对话最应该保留的，通常是一份清楚的任务说明。代码可以让本地 Agent 在真实仓库里写，任务边界必须先由人确认。

## 任务转换模板

让网页模型输出这个格式：

```markdown
# Task Brief

## Goal
要完成什么，为什么要做。

## User / Scenario
谁会用，在什么场景下用。

## Requirements
- 必须实现什么。
- 必须保持什么。

## Out of Scope
- 本次不做什么。

## Context Pointers
- 本地 Agent 应该先读哪些文件或目录。

## Constraints
- 技术、风格、兼容性、安全边界。

## Done When
- 哪些现象、测试、命令证明完成。

## Risks
- 可能出错的地方。
```

这里保留 `Out of Scope` 和 `Done When` 这两个英文域名，是因为它们常作为任务模板字段出现：`Out of Scope` 表示本次明确不做什么，`Done When` 表示可验证的完成条件。

## 给网页模型的提示词

```text
我接下来要把这个需求交给本地 Claude Code / Codex / OpenCode 执行。
请不要写代码。
请把我们的讨论整理成 Task Brief，重点写清楚：
目标、范围、不做什么、上下文入口、约束、验收标准、风险。
输出要适合直接保存为 prd.md。
```

## 交给本地 Agent 前再做一次压缩

网页模型输出通常会偏长。交给本地 Agent 前，压缩成：

- 目标不超过 5 行。
- Requirements 不超过 10 条。
- 必须写清 `Out of Scope`（本次不做什么）。
- Context Pointers 必须是路径或明确对象。
- `Done When`（完成条件）必须可验证。

## 反例

不要把这种东西交给本地 Agent：

```text
帮我做一个像 Notion 一样的工具，要好看，要能登录，要有数据库。
```

这不是任务，是愿望。

更好的版本：

```text
目标：实现一个最小 notes CRUD 页面。
范围：只做本地登录后的 notes 列表、新建、编辑、删除。
不做：协作、分享、富文本、权限系统。
上下文：先读 src/app/notes、src/lib/db、已有 auth middleware。
验收：新增 note 后刷新仍存在；删除后列表消失；相关测试通过。
```

本地 Agent 需要的是可执行任务，不是愿望式描述。任务越小，越容易验证；边界越清楚，越少返工。
