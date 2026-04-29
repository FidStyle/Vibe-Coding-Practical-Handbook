<div data-theme-toc="true"></div>

# 6.7 长任务治理、恢复和交接

> 这一节说明如何把超过半天的 coding agent 任务拆成可恢复、可验证、可交接的工作流。

长任务失败的常见原因不一定是模型能力不足，更多时候是任务边界、状态和验证没有被管理。

## 什么算长任务

满足任意一条就按长任务处理：

- 预计超过 2 小时。
- 涉及 5 个以上文件。
- 涉及 3 个以上模块。
- 需要迁移数据或公共接口。
- 需要多轮调试。
- 需要跨会话继续。
- 需要多个 Agent 并行。

## 长任务的主要风险

| 风险 | 表现 |
|---|---|
| 上下文漂移 | Agent 忘记原目标 |
| 改动过大 | 审查难以推进，无法定位问题 |
| 验证滞后 | 写完才发现基础假设错了 |
| 状态丢失 | 新会话不知道完成了什么 |
| 文件冲突 | 多 Agent 改同一范围 |
| 人工控制失效 | 无法解释 Agent 为什么这样修改 |

## 拆分原则

不要按“代码层”机械拆分，要按“可验证成果”拆分。

差的拆分：

```text
1. 改 types
2. 改 backend
3. 改 frontend
4. 改 tests
```

好的拆分：

```text
1. 建立最小数据模型和测试。
2. 完成后端 API 路径，并用集成测试验证。
3. 接入前端读取路径，不做写入。
4. 接入写入和错误处理。
5. 补端到端验证和文档。
```

每一步都应该能单独审查。

## 任务目录模板

```text
tasks/2026-04-28-import-csv/
├── prd.md
├── plan.md
├── steps.md
├── journal.md
├── decisions.md
└── validation.md
```

| 文件 | 作用 |
|---|---|
| `prd.md` | 目标、需求、非目标、验收 |
| `plan.md` | 总体方案和风险 |
| `steps.md` | 可执行任务拆分 |
| `journal.md` | 会话进展 |
| `decisions.md` | 关键决策和原因 |
| `validation.md` | 跑过的验证和结果 |

## checkpoint 机制

每完成一个子任务，要求 Agent 停下。

```text
完成当前 step 后停止，不要继续下一个 step。
请输出：
- 修改了哪些文件。
- 满足了哪个验收条件。
- 跑了哪些验证。
- 当前风险。
- 下一步建议。
```

不要让 Agent 在你没审查的情况下连续推进多个阶段。长任务的节奏应该由 checkpoint 控制，而不是由模型输出速度控制。

## 会话恢复摘要

长任务每次收尾都生成恢复摘要。

```markdown
# Resume Summary

## Original Goal

[原始目标]

## Current Step

[正在做哪个 step]

## Completed

- [x] Step 1
- [x] Step 2

## Current Diff

[主要改动摘要]

## Validation

- `pnpm test`: passed
- `pnpm typecheck`: failing because ...

## Open Issues

- Issue 1

## Next Action

[下一步只做一件事]
```

新会话只需要读取任务目录和这份摘要，不需要重新粘贴完整聊天记录。

## 多 Agent 并行

并行只适合边界清楚的工作。

适合并行：

- 一个 Agent 写测试，另一个读实现方案。
- 一个 Agent 做前端样式，另一个做后端接口。
- 一个 Agent 做审查，另一个继续非重叠实现。
- 一个 Agent 查文档，另一个整理本地代码模式。

不适合并行：

- 多个 Agent 改同一文件。
- 需求还没定。
- 架构边界不清。
- 需要频繁共享中间状态。

并行前必须写清楚：

```text
Agent A owns files: ...
Agent B owns files: ...
Do not edit files outside your ownership without asking.
```

## worktree 的价值

worktree 解决的是文件冲突和审查隔离。

```text
主工作区
  稳定主工作区

worktree A
  Agent A 做功能实现

worktree B
  Agent B 做测试 / 审查 / 重构
```

worktree 只隔离文件变更，不负责自动拆分任务。任务拆不好，worktree 也只是把混乱分散到多个目录。

## 长任务完成标准

- [ ] 每个 step 都有独立验证。
- [ ] 每次中断都有 journal。
- [ ] 决策记录能解释为什么这么做。
- [ ] 当前 diff 可以审查。
- [ ] 没有跨 Agent 文件冲突。
- [ ] 最终文档和测试同步。
- [ ] 任务结束后沉淀了可复用规则。
